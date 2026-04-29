# Article Support Dataset v1

This bundle assembles the core machine-readable artifacts behind the paper draft.
It includes:

- the full list of 509 repositories in the instruction-surface corpus
- per-repository GitHub metadata snapshots and joined repo-level analysis rows
- sanitized corpus-wide `scan-project` findings output
- summary analysis outputs used in the paper
- raw/sanitized verification and adjudication artifacts for the TP/FP study

## Layout

- `repositories/`: repo list and metadata tables
- `analysis/`: repo-level results, summary analysis JSON, and sanitized corpus findings
- `triage/`: raw/sanitized verification and adjudication exports

## Important notes

- Local filesystem roots were replaced with placeholders such as `<corpus>`, `<workspace>`, and `<agent-audit-repo>`.
- Repository names and origin URLs were intentionally preserved because they are part of the article-support dataset.
- `corpus_project_findings_sanitized.json` is the full corpus-wide `scan-project` output with local paths sanitized, not just a summary.
- Reviewer raw files are included where retained in the workspace. The Opus reviewer export is included from the archived CSV zip.
