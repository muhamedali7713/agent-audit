# Working Paper Draft v9

## Provisional Title

Instruction Surfaces in Agent-Centric Repositories: Static Measurement of
Skills, Agent Guidance, and Tool Integration Artifacts

## Abstract

As coding agents spread into software repositories, maintainers and users
need practical ways to assess whether local agent instructions encourage
unsafe behavior. Repository-local artifacts such as skills, agent guidance
files, plugin manifests, and MCP manifests can change how an agent plans,
delegates, fetches, executes, persists, and interacts with external
systems. To support repository review at scale, we developed `agent-audit`,
an automated static-analysis utility for scanning instruction surfaces and
classifying security-relevant patterns.

We apply `agent-audit` to a purposive corpus of `509` instruction-rich
repositories. The project-level scan produced `4,882` medium-or-higher raw
findings and `4,637` issue instances after conservative clustering by scope
and canonical issue class. Findings are highly concentrated: the top `1%`
of repositories account for `63%` of findings and the top `10%` account for
`90%`, with Gini values near `0.94` under multiple sensitivity cuts.
Instruction-surface density, especially `SKILL.md` count, is the strongest
correlate; the relationship weakens but remains substantial after
normalization by files and instruction lines. Five canonical classes led by
autonomy, execution, instruction override, role manipulation, and
trust-boundary expansion account for about `85%` of clustered issue
instances.

The tool combines native agent-instruction detectors with imported rule
families derived from ASAMM, Cisco prompt-guard style rules, ATR, and
Aguara-style external-action checks. We also ran a three-model LLM
adjudication audit on `54` stratified issue instances. File-backed items
were more stable than category-root aggregate packets in this subset
(`33/46` unanimous versus `1/8`), though the aggregate sample is small. The
final adjudicated reference subset contained `27` TP, `26` FP, and `1`
`UNCERTAIN`, so roughly half of the stage-2 records resolved to FP under
model-based review; because the stage-2 sample oversampled contested,
aggregate, and rare-class cases, this ratio is not a corpus-wide precision
estimate. Large-scale analysis of instruction-rich repositories therefore
needs not only detector coverage, but also separation between raw rule hits,
artifact-backed issue instances, and aggregate signals so that developers
and users can interpret repository risk more practically.

## 1. Introduction

Coding agents increasingly rely on repository-local artifacts that alter
their behavior before any interactive reasoning occurs. Examples include
skills, agent guidance files, path-specific instructions, plugin manifests,
and MCP configuration or integration documents. These artifacts can shape
which tools an agent prefers, what approval model it assumes, what external
systems it touches, and whether changes persist into future sessions. As a
result, the security-relevant instruction surface of a repository is wider
than prompt text alone and often intersects with autonomy, command
execution, configuration mutation, and credential-bearing workflows.

This creates a problem for both practitioners and researchers. For
practitioners, the challenge is to determine which instruction-bearing
artifacts merely document workflows and which meaningfully expand the
agent's trust boundary or operational authority. For researchers, the
challenge is methodological: raw detector hits across skill-heavy
repositories tend to be duplicated, highly skewed, and heterogeneous in
meaning. Without careful clustering and adjudication, apparent prevalence can
be distorted by replicated templates, over-broad detector semantics, or
evidence-poor aggregate packets.

This paper studies repository-local instruction surfaces as a security
phenomenon in a purposive corpus of instruction-rich repositories. Rather
than asking whether "agents are safe" in general, we ask how an automated
repository-audit utility can help identify where risky agent behavior is
most likely to be encoded, replicated, and operationalized. Concretely, we
develop and evaluate `agent-audit`, then use it to characterize which
patterns dominate repositories that explicitly embed agent guidance, skills,
plugins, and MCP artifacts. Our research questions are:

1. How are security findings distributed across instruction-rich
   repositories?
2. Which instruction-surface features are most strongly associated with
   findings and issue instances?
3. What issue classes dominate these repositories once detector overlap is
   reduced?
4. How often do static findings correspond to operationally meaningful risky
   surfaces under model-based adjudication, and where does disagreement
   concentrate?

Our contribution is threefold. First, we present `agent-audit`, an
automated utility for scanning repository-local instruction surfaces with a
combination of native agent-instruction logic and imported detector packs.
Second, we build and characterize a large, purposive corpus of
instruction-rich repositories and distinguish raw rule hits, artifact-backed
issue instances, and aggregate signals, showing that the detector layer is
highly concentrated in skill-dense projects and dominated by autonomy,
execution, instruction override, and trust-boundary expansion classes.
Third, we show through a three-model LLM adjudication audit that evidence
strength and canonical class fit are central to interpretable
agent-security measurement.

## 2. Corpus Construction

The corpus contains `509` repositories organized into four source cohorts:

1. `51` top instruction-centric repositories
2. `25` agent tooling repositories
3. `25` custom agent/instruction/skill collections
4. `408` broader discovery repositories mentioning instruction-surface
   artifacts

This is a purposive rather than random sample. The goal was not to estimate
GitHub-wide prevalence, but to assemble a realistic and diverse set of
repositories where coding-agent behavior is plausibly shaped by local
artifacts. The broad fourth cohort serves as a discovery layer, while the
first three intentionally enrich for repositories where instruction artifacts
are central rather than incidental.

The resulting corpus is instruction-surface rich. Across all repositories,
`418` contain at least one instruction-file surface, `261` contain one or
more `SKILL.md` files, `264` contain `AGENTS.md`, `246` contain `CLAUDE.md`,
`78` contain plugin manifests, and `58` contain MCP manifests. Among the
`261` repositories with at least one `SKILL.md`, the corpus contains
`16,176` skill files total.

This cohort construction licenses claims about instruction-rich repositories
selected for instruction-surface relevance. It does not license claims about
GitHub as a whole, open-source software in general, or all coding-agent
workflows.

The primary corpus composition, project-level scan artifact, and normalized
and sensitivity analysis bundles were finalized on `2026-04-25`. The
repository metadata snapshot `repo-metadata-v1` was collected on
`2026-04-27`.

## 3. Artifact Discovery and Surface-Indicator Characterization

Instruction-bearing skill files in the corpus are not simple documentation
stubs. They frequently contain operationally relevant language and
integration structure. Among repositories with at least one `SKILL.md`,
feature prevalence is high for:

- plugin or MCP linkage: `89%` of repositories
- shell execution language: `90%`
- approval or scoping language: `82%`
- remote fetch patterns: `73%`
- credential references: `66%`
- autonomy-loop language: `77%`
- external-reply actions: `69%`
- write actions: `78%`
- session reuse or persistence language: `65%`
- identity rewrite language: `62%`

These feature rates show that the corpus was not assembled from arbitrary
markdown. It is specifically rich in action-bearing and integration-bearing
instruction artifacts. This supports interpreting the corpus as a study of
agent instruction surfaces rather than of generic documentation. We treat
these feature prevalences as surface indicators, not as confirmed risky
behavior under adjudication. A feature may appear in benign documentation,
examples, quoted attack patterns, or defensive guidance; operational risk is
assessed later through canonical issue clustering and adjudication. We did
not separately audit feature-detector precision, so these rates should be
read as detector hits over surface-indicator classes, not adjudicated
incidence.

Skill-count buckets also show threshold-like behavior. Repositories with `0`
or `1-4` skill files are largely quiet, while repositories with `5-19`,
`20-99`, and especially `100+` skill files exhibit sharp increases in both
finding prevalence and collection-scale patterns. This motivates treating
`skill_files` as an exposure variable throughout the analysis and testing
whether its association with findings survives normalization by scanned
surface.

## 4. Methods

### 4.1 Tool overview and detector sources

The analysis in this paper is performed with `agent-audit`, an automated
repository-audit utility developed to inspect instruction-bearing artifacts
for security-relevant surfaces before repositories are reused, extended, or
trusted in agent-centric workflows. The tool is intended to be useful for
both developers and downstream users: developers can use it to find risky
instruction patterns in their own repositories, while users can use it to
triage third-party agent repos before adoption.

`agent-audit` combines two detector layers. The first is a native layer
designed specifically for agent-instruction analysis. These rules focus on
structural gaps such as broad external action without approval, autonomous
loops with writes, and persistent identity rewrite. The second is an
imported rule-pack layer that broadens coverage with external detector
families, including Cisco prompt-guard style rules for prompt injection and
PII exposure, ATR-style autonomy/manipulation classes, and Aguara-style
checks for external downloads and related trust-boundary expansion
patterns. Imported rules are not treated as ground truth; instead, they are
harmonized into a common severity and canonical-class framework, then
evaluated together with native findings and post-processing logic.

### 4.2 Detection model and analysis views

We analyzed each repository using a project-level static scanning pipeline
that produces two principal views.

The first is the raw detector view. This includes:
- native structural findings developed specifically for agent-instruction
  analysis
- imported rule-pack findings from multiple external sources
- collection-scale aggregation where the same pattern replicates across a
  large cohort of sibling artifacts

The second is an issue-centric clustered view. In this representation,
findings are clustered conservatively by `(scope_key, canonical_class)`,
producing deduplicated issue instances that better reflect the fact that
multiple rules often describe different facets of the same risky surface.
The clustering is intentionally narrow: it does not aggressively merge across
files or across repositories, and it is therefore expected to reduce raw
counts only modestly.

Table 1 summarizes the canonical classes used in the clustered view. These
classes are the analytic targets of the issue-centric layer, not merely
informal labels applied after the fact.

| Canonical class | Primary source families | Risk definition | Typical evidence basis | Common FP mode |
| --- | --- | --- | --- | --- |
| `autonomous_execution_or_looping` | `asamm.AD-02.autonomous-loop-with-writes`, `atr.excessive-autonomy` | Open-ended or repeated execution, especially when paired with writes or unattended continuation | Structural cues plus autonomy language and write actions | Bounded CI workflows, dry-run examples, or narrative planning text |
| `unsafe_command_or_execution_surface` | `atr.privilege-escalation` | Shell, eval, or command-composition surfaces that can broaden execution authority or permit unsafe interpolation | Exact-pattern and structural matches | Quoted shell syntax, educational examples, or defensive exploit discussion |
| `prompt_or_instruction_override_surface` | `atr.prompt-injection` | Explicit override, disclosure, or replacement of higher-priority instructions | Exact-pattern and lexical matches | Benchmark prompts, attack examples, or warning text describing the pattern |
| `agent_role_or_goal_manipulation` | `atr.agent-manipulation` | Role, authority, or objective reframing that can shift agent behavior | Lexical and structural matches | Taxonomy prose, defensive guidance, or non-operative discussion of roles |
| `remote_fetch_or_install_expands_trust_boundary` | `aguara.external-download` | Remote fetch, install, or auto-registration behavior that broadens the agent's trust boundary | Exact-pattern plus integration structure | Install snippets, package-manager examples, or setup instructions without autonomous use |
| `broad_external_action_without_approval` | `asamm.AD-02.broad-action-without-approval` | Broad remote or external action capability without clear approval/scoping semantics | Structural evidence over manifests and skill text | Capability inventories or integration guides that mention actions but not active execution defaults |
| `identity_rewrite_with_persistent_effect` | `asamm.AI-04.persistent-identity-rewrite` | Future-session identity, role, or policy rewrite with persistent effect | Structural linkage between rewrite language and persistent config/write path | Benign persona/project guidance without real persistence effect |
| `credential_or_pii_exposure_surface` | `cisco-pg.pii_exposure` | Credential or PII collection/exposure surface | Exact-pattern lexical cues | Mock values, redaction examples, or defensive handling guidance |
| `tool_or_skill_poisoning_surface` | `atr.tool-poisoning`, `atr.skill-compromise` | Manipulation or compromise of tool, skill, or control-plane inputs | Exact-pattern and structural cues | Defensive exploit discussion or quoted attack payloads |
| `ssrf_or_internal_service_reachability` | `aguara.ssrf-cloud` | Reachability of metadata or internal services consistent with SSRF-style abuse | Exact-pattern matches | Defensive references to metadata endpoints without exploit flow |

The primary scan artifact for the corpus contains `4,882` medium-or-higher
raw findings and `4,637` clustered issue instances. Native structural
findings contribute `88` findings across `22` repositories, but they
represent higher-precision classes such as broad external action without
approval, autonomous loops with writes, and persistent identity rewrite. The
headline `medium-or-higher` count uses the scanner's normalized severity
field after ingestion into a common ordinal scale. Native findings emit
severity directly on that scale, while imported rule-pack severities are
preserved where available and mapped into the same `medium/high/critical`
reporting bands.

Table 2 highlights the three main evidence layers used later in the paper.
These layers are not identical in semantics or intended interpretation and
should not be collapsed into a single notion of "finding quality."

| Evidence layer | Raw count | Repository coverage | Adjudication role | Intended interpretation |
| --- | ---: | ---: | --- | --- |
| Native structural findings | `88` | `22` repos | Directly represented in the adjudication subset | Higher-precision structural control gaps tailored to agent-instruction artifacts |
| Collection-scale aggregate signals | `270` | `71` repos | Included as `category-root` packets in the adjudication subset | Replicated pattern families across sibling artifacts; weaker as stand-alone evidence units |
| Imported rule-pack findings | `4,526` non-aggregate raw findings | `182` repos with at least one reported finding overall | Present both directly and as support for clustered issue instances | Broad-coverage detector layer useful for surfacing candidate risky surfaces, but heterogeneous in semantics and precision |

These evidence-layer counts are not fully mutually exclusive. In particular,
two native findings are also collection-scale aggregate signals, so Table 2
is intended as an interpretation guide rather than a partition of the raw
finding total.

### 4.3 Repository-level enrichment

At the repository level, we enriched the scan artifact with:

- instruction-surface inventory counts
- skill counts and surface diversity
- plugin/MCP manifest presence
- maintenance/activity covariates:
  `last_commit_at`, `days_since_last_commit`, `stale_bucket`,
  `recently_active_90d`
- popularity/status covariates:
  `stars`, `forks`, `archived`, and `license` when available

Metadata coverage for repository-level enrichment is:
- `stars`: `507/509`
- `forks`: `507/509`
- `archived`: `509/509`
- `license`: `452/509`

### 4.4 Normalized outcomes

Because the number of instruction artifacts can mechanically increase
detector opportunity, we report not only raw counts but also normalized
outcomes:

- findings per instruction-surface file
- clustered issue instances per instruction-surface file
- findings per skill file
- clustered issue instances per skill file
- findings per 1,000 instruction-surface lines
- clustered issue instances per 1,000 instruction-surface lines

These outcomes are used to test whether the `skill_files` relationship is
only a denominator effect or whether it persists after scaling by scanned
surface.

### 4.5 Sensitivity scenarios

To test the robustness of the heavy-tail result, we constructed a
sensitivity pack with four scenarios:

1. excluding the top `1%` of repositories by findings
2. excluding the top `2` repositories by findings
3. excluding collection-scale aggregate signals
4. deduplicating repeated upstream repositories across categories

These scenarios were applied to both raw findings and clustered issue
instances.

### 4.6 Adjudication audit

To evaluate interpretive stability, we constructed an adjudication subset of
`54` canonical issue instances and obtained three independent LLM reviewer
opinions using frontier-model interfaces labeled `GPT-5.5`, `Claude Opus 4.7`,
and `Codex desktop`. We report the exact interface labels exposed in the
adjudication environment; stable API model strings were not available for
all three interfaces. This is therefore not a human inter-rater-reliability
study. Instead, it is a cross-model
adjudication exercise intended to measure stability under frontier-model
review, while acknowledging that the models may share training data,
heuristics, and correlated failure modes.

The adjudication subset was not random. We first built a reviewer-ready
packet of `92` records, then defined the adjudication subset as all records
marked `stage2_target`, yielding `54` cases. This stage-2 subset was
stratified for methodological value rather than prevalence estimation. It
intentionally included all `category-root` aggregate records, all
cross-tool-corroborated cases, rare persistent-identity-rewrite cases,
native autonomy cases, and targeted top-ups for broader canonical-class
coverage. The resulting sample is well suited for stress-testing
interpretive ambiguity, but it should not be read as an unbiased estimate of
corpus-wide detector precision.

Each reviewer labeled every record as `TP`, `FP`, or `UNCERTAIN`. We then
computed:

- pairwise percent agreement
- Cohen's kappa
- unanimity versus split counts
- file-level versus aggregate stability

For the `20` non-unanimous cases, we also performed a deeper follow-up
assessment to construct a final adjudicated reference subset. That reference subset should
be interpreted as a best-effort adjudicated subset rather than a perfect
ground truth, because it inherits any systematic blind spots shared by the
initial LLM reviewers.

### 4.7 Reproducibility and artifact release

The analysis is accompanied by a reproducibility bundle containing the
corpus manifest, primary scan artifacts, clustered issue outputs,
repository-level metadata snapshot, normalized-outcome metrics, sensitivity
scenarios, adjudication packets, and figure-generation scripts. The study is
therefore reproducible at the level of corpus composition, static scan
outputs, derived metrics, and model-based adjudication tables, even though
it does not provide runtime exploit validation for every finding.

## 5. Results

### 5.1 Distribution of findings

The corpus is highly concentrated. At the repository level, the median
finding count is `0`, and the Gini coefficient is approximately `0.94` for
both raw findings and clustered issue instances. Roughly the top `1%` of
repositories account for `63%` of all findings, while the top `10%` account
for `90%`. This pattern remains after issue clustering, which means that the
heavy tail is not primarily a rule-overlap artifact. Instead, a small set of
repositories dominate the finding landscape and candidate security-relevant
surface area because they contain extremely dense and repetitive instruction
surfaces.

### 5.2 Cohort differences

The curated instruction-centric cohorts differ materially from the broader
discovery cohort. Across the three curated categories, the clustered-issue
repository rate is `0.634` (approximate 95% CI `0.54-0.73`), compared with
`0.289` in the broad-surface cohort (approximate 95% CI `0.25-0.33`). Native
finding rates are even more uneven: `0.139` in curated cohorts (approximate
95% CI `0.07-0.21`) versus `0.020` in the broad cohort (approximate 95% CI
`0.01-0.03`). We report approximate binomial CIs to characterize variation
within this corpus, not to make inferential claims about a wider population.
Within that bounded sense, the corpus construction strategy appears to have
successfully enriched for repositories where agent-instruction surfaces
contain higher-signal structural findings.

### 5.3 Instruction-surface density and normalized outcomes

Instruction-surface density is the strongest correlate of findings. The
number of `SKILL.md` files has Spearman `rho = 0.780` with total findings
and `rho = 0.779` with clustered issue instances. Total instruction-surface
files are also strongly associated with both outcomes (`rho = 0.743` in both
cases).

However, this raw relationship is partly an exposure effect: more
instruction-bearing artifacts create more opportunities for detectors to
fire. For that reason we also evaluated normalized outcomes. After
normalization, the `skill_files` relationship weakens but remains
substantial:

- findings per instruction-surface file: `rho = 0.634`
- clustered issue instances per instruction-surface file: `rho = 0.631`
- findings per skill file: `rho = 0.479`
- clustered issue instances per skill file: `rho = 0.474`
- findings per 1,000 instruction-surface lines: `rho = 0.620`
- clustered issue instances per 1,000 instruction-surface lines:
  `rho = 0.617`

This is the strongest defensible interpretation of the `skill_files` result:
instruction-surface density behaves as a strong exposure gradient, and a
substantial positive association remains even after scaling by scanned
surface. Skill-count buckets still show threshold-like escalation under this
view, although less dramatically than in the raw count analysis.

### 5.4 Dominant issue classes

The canonical issue layer shows a stable finding mix dominated by:

- `autonomous_execution_or_looping`: `1,155`
- `unsafe_command_or_execution_surface`: `1,004`
- `prompt_or_instruction_override_surface`: `781`
- `agent_role_or_goal_manipulation`: `573`
- `remote_fetch_or_install_expands_trust_boundary`: `419`

These counts indicate that the main picture in the corpus is not simply
credential exposure or isolated prompt-injection folklore. Instead, the
dominant patterns involve autonomy, command and execution surfaces,
instruction override, role or goal manipulation, and expansion of the trust
boundary via remote fetch or installation. Together, these five classes
account for roughly `85%` of clustered issue instances (`3,937/4,637`). The
remaining five canonical classes are `tool_or_skill_poisoning_surface`
(`369`), `credential_or_pii_exposure_surface` (`216`),
`broad_external_action_without_approval` (`64`),
`ssrf_or_internal_service_reachability` (`37`), and
`identity_rewrite_with_persistent_effect` (`11`).

### 5.5 Popularity and maintenance covariates

Popularity is positively associated with findings, but only moderately:

- `stars -> findings_total`: `rho = 0.223`
- `stars -> cluster_issue_instances`: `rho = 0.224`
- `forks -> findings_total`: `rho = 0.227`
- `forks -> cluster_issue_instances`: `rho = 0.229`

These relationships are much weaker than the `skill_files` association,
which suggests that popularity alone does not fully account for the heavy
tail.

Maintenance/activity covariates are consistent with a corpus that is largely
active rather than dominated by abandoned projects.
The correlation between `days_since_last_commit` and total findings is
`-0.201`, and the corresponding value for clustered issue instances is also
`-0.201`. Most repositories are active (`475` active within `90` days), and
only `4` are in the `365d+` stale bucket. Archived status is almost
degenerate (`1` archived repository), so it functions mainly as a sanity
check showing that the corpus is not dominated by archived projects.

### 5.6 Sensitivity analysis

The heavy-tail result survives all major robustness cuts.

Baseline concentration is strong in both views:

- findings Gini: `0.940`
- clustered-issue Gini: `0.940`

After excluding the top `1%` of repositories, concentration weakens but
remains high:

- findings Gini: `0.868`
- clustered-issue Gini: `0.866`

After excluding the top `2` repositories:

- findings Gini: `0.885`
- clustered-issue Gini: `0.884`

Removing collection-scale signals does not materially reduce concentration:

- findings Gini: `0.943`
- clustered-issue Gini: `0.942`

Likewise, deduplicating repeated upstream repositories across categories has
little effect:

- findings Gini: `0.944`
- clustered-issue Gini: `0.943`

Taken together, these sensitivity results show that the heavy tail is not
explained away by duplicate upstream repos, category-root aggregate signals,
or a single top repository. Trimming the very top tail reduces concentration,
as expected, but the corpus remains strongly skewed.

## 6. Evaluation: Adjudication and Interpretive Stability

The adjudication audit produced `54` reviewed records, of which `34` were
unanimous and `20` split `2-1`; there were no `1-1-1` cases. In practice,
`UNCERTAIN` was used sparingly by two of the three reviewers (`3`, `1`, and
`8` `UNCERTAIN` labels respectively), so the adjudication behaved mostly as
a binary disagreement task with a smaller third label rather than as a fully
balanced three-class annotation exercise.

Pairwise reviewer agreement under multi-model LLM adjudication was:

- `GPT-5.5` vs `Claude Opus 4.7`: agreement `0.815`, kappa `0.656`
- `GPT-5.5` vs `Codex desktop`: agreement `0.685`, kappa `0.476`
- `Claude Opus 4.7` vs `Codex desktop`: agreement `0.759`, kappa `0.584`

The strongest methodological result appears when separating file-backed
records from aggregate packets, although the aggregate subset is small:

- `repo-file`: `46` records, `33` unanimous, `13` split
- `category-root`: `8` records, `1` unanimous, `7` split

The majority-label distribution also differs sharply:

- `repo-file`: majority `TP/FP/UNCERTAIN = 26/20/0`
- `category-root`: majority `TP/FP/UNCERTAIN = 1/4/3`

Table 3 summarizes the same split in compact form.

| Evidence type | n | Unanimous | Split | Majority TP | Majority FP | Majority UNCERTAIN |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| `repo-file` | `46` | `33` | `13` | `26` | `20` | `0` |
| `category-root` | `8` | `1` | `7` | `1` | `4` | `3` |

After deep assessment of the disputed subset, the final adjudicated reference subset
contains:

- `27` TP
- `26` FP
- `1` UNCERTAIN

Excluding the single `UNCERTAIN` case, `26/53` records in this subset
resolve to `FP`, reinforcing that the adjudication packet was useful
precisely because it oversampled cases where detector output is difficult to
interpret cleanly.

This evaluation supports two cautious claims. First, file-backed repository
findings in the adjudicated subset are stable enough to support a serious
interpretation story under multi-model LLM review. Second, the same
adjudicated subset shows a pattern consistent with category-root aggregate
packets being methodologically weaker units, but the aggregate sample is
small (`n = 8`) and the result should therefore be read as suggestive rather
than conclusive.

## 7. Discussion

The data support a clear empirical story. Security-relevant instruction
surfaces are not uniformly distributed across repositories. Instead, they
are concentrated in a relatively small set of skill-dense, instruction-rich
projects. This concentration persists after issue clustering, survives
multiple sensitivity cuts, and is only partly reduced when outcomes are
normalized by scanned surface.

The dominant patterns also matter. The highest-signal findings in this
corpus are not primarily driven by mere mentions of security topics.
Instead, many of the relevant instruction artifacts explicitly describe,
recommend, configure, or expose:

- unattended loops
- command execution
- tool and plugin extension
- prompt or instruction override
- remote action surfaces

This matters for both ecosystem governance and tool design. Repository
maintainers may need to treat instruction-bearing files as part of the
operative security boundary of the project, with caveats about the precision
of current detectors at scale. Tool builders should distinguish:

- raw rule hits
- artifact-backed issue instances
- aggregate architectural signals

The adjudication audit reinforces this point. Some apparent false positives
were not detector failures in the narrow sense, but mismatches between the
finding, the class, and the way the evidence was packaged. The common false-
positive modes listed in Table 1 showed up repeatedly in review: quoted
attack text, defensive prose, capability inventories, install snippets, and
setup-oriented integration guides. In other words, interpretation quality
depends as much on issue modeling and evidence strength as on detector
recall.

## 8. Threats to Validity

The strongest threat is corpus construction. The repository set is purposive,
not random, and is explicitly enriched for instruction-surface artifacts.
This is appropriate for studying instruction-rich repositories, but it rules
out claims about GitHub-wide prevalence.

A second threat is mixed signal semantics. The corpus includes native
structural findings, imported rule-pack findings, and collection-scale
aggregates. These layers are not equivalent in precision or meaning, so raw
counts should not be treated as interchangeable with clustered issue
instances or native structural gaps.

A third threat is residual detector-opportunity confounding. The normalized
outcomes analysis substantially reduces, but does not eliminate, the concern
that more scanned instruction artifacts mechanically yield more findings. The
paper therefore treats `skill_files` primarily as an exposure gradient rather
than as a direct causal variable.

A fourth threat concerns evidence packaging. The adjudication exercise shows
that aggregate packets are weaker units than file-backed artifacts in the
reviewed subset, but the aggregate sample is small and does not justify a
strong population-level stability claim. Stronger claims should therefore be
tied to artifact-backed issue instances or to adjudicated subsets, not only
to category-root aggregate signals.

We also do not estimate precision, recall, or prevalence for the full corpus
from the adjudication subset. The `54` adjudicated records were selected to
stress interpretive ambiguity and class coverage, so their `TP/FP/UNCERTAIN`
balance should not be generalized to all findings in the corpus.

A fifth threat is metadata collection. Repository popularity and status
fields were collected via HTML parsing rather than the GitHub API. Coverage
is strong enough for descriptive use, but the method is still snapshot-based
and parser-dependent. `archived` is too sparse to serve as a serious
statistical predictor, and `license` is incomplete.

A sixth threat is the lack of general runtime validation. This study is
primarily static and artifact-based. It can characterize surfaces and
patterns, but it does not prove exploitability for every finding.

Finally, the study depends partly on imported rule packs and local
post-processing choices. This is a strength for broad coverage, but also a
threat because upstream regexes, imported severity defaults, and downstream
suppression, clustering, and reporting logic all shape the final result.
Imported rule-pack findings and native structural findings should therefore
not be treated as identical in semantics or precision.

A final threat concerns adjudication modality. The three-model exercise
used frontier LLMs rather than human annotators. Agreement metrics therefore
reflect cross-model stability under LLM adjudication, not classical human
IRR. Shared priors, shared benchmark exposure, and correlated failure modes
may inflate agreement on some classes and suppress disagreement on others.
This is still informative for tool-facing and LLM-facing review workflows,
but it does not directly generalize to human-only adjudication.

## 9. Conclusion

In a purposive corpus of `509` instruction-rich repositories, findings are
highly concentrated in a small number of skill-dense projects. The detector
layer is dominated by canonical classes covering autonomy, execution,
instruction override, and trust-boundary expansion. These patterns remain
strongly associated with instruction-surface density even after descriptive
adjustment for repository popularity and maintenance and after normalization
by scanned surface.

The methodological implication is equally important: security analysis of
agent-instruction ecosystems should not stop at raw detector hits. It should
separate artifact-backed issue instances from aggregate architectural
signals, account for canonical class fit, and validate interpretation
through model-based or human adjudication where feasible, including explicit
disclosure when that adjudication is model-based rather than human.
