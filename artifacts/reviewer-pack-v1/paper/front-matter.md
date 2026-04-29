# Camera-Ready Front Matter

## Final Title

Instruction Surfaces in Agent-Centric Repositories: Measuring Skills, Agent
Guidance, and Tool Integration Artifacts

## Short Title

Instruction Surfaces in Agent Repositories

## Camera-Ready Abstract

Repository-local agent artifacts such as `SKILL.md`, `AGENTS.md`,
`CLAUDE.md`, Copilot instruction files, plugin manifests, and MCP manifests
can alter how coding agents plan, delegate, fetch, execute, persist, and
interact with external systems. We study these artifacts as a security
surface in a purposive corpus of `509` instruction-rich repositories. A full
project-level static scan produced `4,882` medium-or-higher raw findings and
`4,637` issue instances after conservative clustering by scope and canonical
issue class.

Findings are highly concentrated: the top `1%` of repositories account for
`63%` of findings and the top `10%` account for `90%`, with Gini values near
`0.94` under multiple sensitivity cuts. Instruction-surface density,
especially `SKILL.md` count, is the strongest correlate; the relationship
weakens but remains substantial after normalization by files and instruction
lines. Five canonical classes led by autonomy, execution, instruction
override, role manipulation, and trust-boundary expansion account for about
`85%` of clustered issue instances, while popularity and maintenance
covariates are only moderately associated.

We also ran a three-model LLM adjudication audit on `54` stratified issue
instances. File-backed items were more stable than category-root aggregate
packets in this subset (`33/46` unanimous versus `1/8`), though the
aggregate sample is small. The final adjudicated reference subset contained
`27` TP, `26` FP, and `1` `UNCERTAIN`; because the stage-2 sample
oversampled contested, aggregate, and rare-class cases, these counts are not
corpus-wide precision estimates. Large-scale analysis of instruction-rich
repositories should distinguish raw rule hits, artifact-backed issue
instances, and aggregate signals, and pair static measurement with
adjudication-aware interpretation.

## Keywords

- coding agents
- agent security
- repository mining
- static analysis
- skills and instruction files
- MCP and plugin ecosystems
- prompt and instruction surfaces
- model-based adjudication

## One-Sentence Contribution

We show that security-relevant instruction surfaces in instruction-rich
repositories are highly concentrated, dominated by a small number of
canonical issue classes, and methodologically interpretable only when raw
rule hits are separated from artifact-backed issue instances and aggregate
signals.
