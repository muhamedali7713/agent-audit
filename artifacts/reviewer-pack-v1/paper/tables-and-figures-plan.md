# Tables and Figures Plan

## Goal

This note lists the core tables and figures needed for a submission-ready
version of the paper. The emphasis is on artifacts that directly support the
main claims and pre-empt the reviewer concerns already raised.

## Core Tables

### Table 1. Canonical Class Taxonomy

Purpose:
- define the issue taxonomy
- show source families, evidence basis, and common FP modes

Source:
- in-paper table already embedded in
  `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/paper-main-draft-v8-2026-04-27.md`

Status:
- complete

### Table 2. Evidence Layers

Purpose:
- separate native structural findings, collection-scale aggregate signals,
  and imported rule-pack findings
- make it clear that these layers differ in semantics and intended use

Recommended columns:
- `Evidence layer`
- `Raw count`
- `Repository coverage`
- `Adjudication role`
- `Intended interpretation`

Source:
- embedded in the paper draft

Status:
- complete

### Table 3. Adjudication Summary by Evidence Type

Purpose:
- make the `repo-file` versus `category-root` split visually obvious

Recommended columns:
- `Evidence type`
- `n`
- `Unanimous`
- `Split`
- `Majority TP`
- `Majority FP`
- `Majority UNCERTAIN`

Source:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/adjudication_tables_report.md`
- embedded in the paper draft

Status:
- complete

### Table 4. Corpus Construction and Coverage

Purpose:
- give readers one compact reference for corpus size and cohort composition

Recommended columns:
- `Cohort`
- `Repositories`
- `Selection intent`
- `Instruction-surface emphasis`

Rows:
- top instruction-centric repositories
- agent tooling repositories
- custom skill/instruction collections
- broader discovery cohort
- total

Status:
- should be added explicitly as a paper table

### Table 5. Key Distributional and Correlation Results

Purpose:
- central numeric results in one place

Recommended columns:
- `Metric`
- `Value`
- `Interpretation`

Recommended rows:
- `Raw findings`
- `Clustered issue instances`
- `Top 1% share`
- `Top 10% share`
- `Findings Gini`
- `Clustered-issue Gini`
- `skill_files -> findings_total`
- `skill_files -> clustered_issue_instances`
- `skill_files -> findings_per_skill_file`
- `skill_files -> clustered_issues_per_skill_file`
- `stars -> findings_total`
- `days_since_last_commit -> findings_total`

Sources:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/mass-scan-publication-analysis-repo-metadata-v1/correlations.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/normalized-outcomes-v1/normalized_correlations.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/sensitivity-pack-v1/sensitivity_scenarios.json`

Status:
- not yet materialized as a clean paper table

## Core Figures

### Figure 1. Findings Concentration Curve

Purpose:
- visualize the heavy tail directly

Plot:
- x-axis: cumulative share of repositories
- y-axis: cumulative share of findings
- lines:
  - raw findings
  - clustered issue instances

Optional overlays:
- line with top `1%`
- line with top `10%`

Interpretation:
- show that concentration is extreme and survives issue clustering

### Figure 2. Sensitivity of Concentration

Purpose:
- show that heavy-tail concentration is not a trivial artifact

Plot:
- bar chart or dot plot of Gini values across scenarios

Rows:
- baseline
- exclude top `1%`
- exclude top `2`
- exclude collection-scale
- upstream dedup

Interpretation:
- make the robustness argument visual

### Figure 3. Skill Exposure and Normalized Outcomes

Purpose:
- answer the mechanical-confounding concern

Plot options:
- boxplot or point-range by `skill_files` bucket for:
  - findings per skill file
  - clustered issues per skill file
- or scatter with log-scaled x-axis and smoothed trend

Preferred buckets:
- `0`
- `1-4`
- `5-19`
- `20-99`
- `100+`

Interpretation:
- show that the signal weakens but does not disappear under normalization

### Figure 4. Canonical Class Composition

Purpose:
- show what kinds of patterns dominate

Plot:
- horizontal bar chart of all `10` canonical classes

Preferred annotation:
- percent of clustered issue instances

Interpretation:
- show that the top five classes account for about `85%` of issue instances

### Figure 5. Cohort Comparison

Purpose:
- show that curated cohorts are not just arbitrary partitions

Plot:
- grouped bars for:
  - clustered-issue repo rate
  - native-finding repo rate

Groups:
- curated combined
- broad discovery

Optional:
- approximate CI whiskers

Interpretation:
- show that enrichment worked and that the broad cohort behaves differently

### Figure 6. Adjudication Stability by Evidence Type

Purpose:
- make the artifact-backed versus aggregate-packet distinction memorable

Plot:
- stacked bars or mosaic plot for `repo-file` and `category-root`

Possible encodings:
- unanimous vs split
- majority TP / FP / UNCERTAIN

Interpretation:
- communicate that aggregate packets are weaker evidence units in the
  reviewed subset

## Optional Figures

### Figure 7. Popularity Versus Findings

Purpose:
- show that popularity does not explain the main pattern

Plot:
- log-scale scatter of `stars` versus `cluster_issue_instances`
- optional trend line

Interpretation:
- moderate association, much weaker than `skill_files`

### Figure 8. Activity Versus Findings

Purpose:
- show that the corpus is mostly active and not just stale artifacts

Plot:
- `stale_bucket` versus clustered-issue repo rate
- or a density plot of `days_since_last_commit`

Interpretation:
- supportive, not central

## Recommended Order in the Paper

1. Table 4: Corpus construction and coverage
2. Figure 1: Findings concentration curve
3. Figure 3: Skill exposure and normalized outcomes
4. Figure 4: Canonical class composition
5. Table 1: Canonical class taxonomy
6. Figure 5: Cohort comparison
7. Figure 2: Sensitivity of concentration
8. Table 2: Evidence layers
9. Table 3 / Figure 6: Adjudication stability by evidence type

## Minimum Submission Set

If the venue limits figures/tables, the minimum high-value set is:

- Table 1: Canonical class taxonomy
- Figure 1: Findings concentration curve
- Figure 3: Skill exposure and normalized outcomes
- Figure 4: Canonical class composition
- Table 3: Adjudication summary by evidence type

That set covers the paper's three strongest pillars:
- concentration
- exposure-normalized structure
- evidence-type-aware adjudication
