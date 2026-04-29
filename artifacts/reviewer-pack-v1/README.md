# Reviewer Pack v1

This pack is a lightweight companion bundle for paper review.

It is designed to be readable in a browser and small enough to inspect quickly,
without shipping the full raw corpus package in the same folder.

## Included

### `paper/`
- `manuscript.md`: current working paper draft
- `front-matter.md`: title / abstract / keywords block
- `tables-and-figures-plan.md`: table and figure map
- `artifact-appendix-and-reproducibility-note.md`: artifact note used by the paper

### `figures/`
- six SVG figures referenced by the manuscript

### `analysis/`
- summary-level corpus outputs for the 509-repository `scan-project` run
- concentration, correlation, cohort, surface, and activity summaries
- human-readable `publication_report.md`

### `triage/`
- lightweight adjudication outputs for the TP/FP study
- trio summary, adjudicated reference subset, and disagreement follow-up tables

## Where to find the full data

For the full machine-readable article-support dataset, including:
- the complete 509-repository list
- per-repository metadata tables
- sanitized corpus-wide `scan-project` findings
- verification and adjudication raw exports

see:

- `../article-support-dataset-v1/`

That bundle is intentionally larger and more analysis-oriented. This reviewer pack
keeps only the manuscript-facing subset.
