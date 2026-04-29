# Artifact Appendix and Reproducibility Note

## Scope

This appendix describes the artifacts needed to reproduce the paper's main
tables and claims. It is intended as a paper-facing appendix, not as an end-
user guide.

## Reproducibility Claim

The paper is reproducible at the level of:
- corpus composition
- static scan outputs
- issue clustering outputs
- repository-level metadata snapshot
- normalized-outcome analysis
- sensitivity analysis
- model-based adjudication tables

The paper is not reproducible at the level of:
- GitHub ecosystem prevalence beyond this purposive corpus
- runtime exploitability of every finding
- human-only adjudication outcomes

## Core Input Corpus

Primary corpus root:
- `/Users/serg/Documents/ASAMM/agent-instruction-corpus/clones`

Corpus size used in the paper:
- `509` repositories

Cohorts:
- `51` top instruction-centric repositories
- `25` agent tooling repositories
- `25` custom agent/instruction/skill collections
- `408` broader discovery repositories

Important note:
- the corpus is purposive and instruction-surface enriched
- claims in the paper are scoped to instruction-rich repositories selected
  for instruction-surface relevance

## Primary Scan Dataset

Main dataset used for corpus-level analysis:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/mass-scan-dedup-v1/project-findings.json`

Supporting sidecars:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/mass-scan-dedup-v1/clustered-findings.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/mass-scan-dedup-v1/security-profile.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/mass-scan-dedup-v1/report-profiles.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/mass-scan-dedup-v1/files-of-concern.json`

Human-readable summary:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/mass-scan-dedup-v1/project-findings.md`

## Repository-Level Metadata Snapshot

Paper snapshot name:
- `repo-metadata-v1`

Files:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/repo-metadata-v1/repo-metadata-v1.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/repo-metadata-v1/repo-metadata-v1-summary.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/repo-metadata-v1/repo-metadata-v1-paper-note.md`

Coverage:
- `stars`: `507/509`
- `forks`: `507/509`
- `archived`: `509/509`
- `license`: `452/509`

Collection method:
- HTML-derived GitHub metadata snapshot

Interpretation limits:
- suitable for descriptive covariates
- not ideal for high-stakes platform-state inference

## Main Publication Analysis Bundle

Primary publication analysis:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/mass-scan-publication-analysis-repo-metadata-v1/publication_report.md`

Supporting outputs:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/mass-scan-publication-analysis-repo-metadata-v1/correlations.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/mass-scan-publication-analysis-repo-metadata-v1/top_repos.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/mass-scan-publication-analysis-repo-metadata-v1/repo_metrics.json`

This bundle supports:
- concentration claims
- cohort comparisons
- popularity/activity covariates
- canonical-class prevalence

## Normalized Outcomes Bundle

Files:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/normalized-outcomes-v1/normalized_outcomes_report.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/normalized-outcomes-v1/normalized_correlations.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/normalized-outcomes-v1/normalized_repo_metrics.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/normalized-outcomes-v1/normalized_skill_bucket_summary.json`

This bundle supports:
- exposure-gradient interpretation
- rebuttal to the detector-opportunity confounding critique

## Sensitivity Analysis Bundle

Files:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/sensitivity-pack-v1/sensitivity_report.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/sensitivity-pack-v1/sensitivity_scenarios.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/sensitivity-pack-v1/duplicate_upstream_groups.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/sensitivity-pack-v1/upstream_dedup_rows.json`

This bundle supports:
- robustness of the heavy-tail result
- sensitivity to top-tail trimming
- sensitivity to collection-scale removal
- sensitivity to duplicate-upstream deduplication

## Adjudication Artifacts

Main adjudication analysis:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/adjudication_tables_report.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/adjudication_summary.json`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/pairwise_agreement.csv`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/scope_type_adjudication.csv`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/canonical_class_adjudication.csv`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/adjudication-tables-v1/final_adjudicated_gold_set.csv`

Reviewer packets:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/tpfp-review-packet-v2.zip`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/datasets/tpfp-adjudication-subset-v2.zip`

Important methodological note:
- this is a cross-model LLM adjudication exercise
- it is not a human IRR study
- the adjudicated subset is stratified for methodological value and should
  not be interpreted as an unbiased estimate of full-corpus precision

## Source Drafts and Revision Trail

Current paper draft:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/paper-main-draft-v8-2026-04-27.md`

Prior drafts:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/paper-main-draft-v2-2026-04-27.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/paper-main-draft-v3-2026-04-27.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/paper-main-draft-v4-2026-04-27.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/paper-main-draft-v5-2026-04-27.md`

Review-response notes:
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/peer-review-response-and-revision-plan-2026-04-27.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/peer-review-2-response-and-revision-plan-2026-04-27.md`
- `/Users/serg/Documents/ASAMM/codex-audt-fresh-2026-04-25/analysis/peer-review-3-response-and-revision-plan-2026-04-27.md`

## Recommended Artifact Submission Contents

Minimum reproducibility bundle for a paper artifact release:
- corpus manifest
- `project-findings.json`
- `clustered-findings.json`
- `security-profile.json`
- `repo-metadata-v1.json`
- normalized outcomes bundle
- sensitivity pack
- adjudication tables bundle
- reviewer packet schemas/guidelines
- current paper draft
- tables-and-figures plan

## Minimal Reproduction Workflow

1. Load the corpus manifest and verify repository count.
2. Re-run the project-level scan to regenerate `project-findings.json`.
3. Rebuild clustered issue instances and security profiles.
4. Reattach `repo-metadata-v1` or rebuild metadata snapshot.
5. Recompute:
   - publication analysis
   - normalized outcomes
   - sensitivity pack
   - adjudication tables
6. Compare regenerated outputs against the stored JSON and markdown reports.

## Limits of the Artifact

The artifact is sufficient to reproduce the paper's descriptive and
methodological analysis. It is not sufficient to prove:
- runtime exploitability of any individual finding
- population prevalence outside the purposive corpus
- human-annotated ground truth independent of the model-based adjudication

That limitation is part of the paper's claims, not a hidden weakness of the
bundle.
