# Public Mass Scan Summary v1

This bundle contains **summary-level public artifacts** from the `scan-project`
run over a 509-repository instruction-surface corpus.

## Included

- `publication_report.md`: main narrative summary
- `summary.json`: compact headline metrics
- `canonical_class_summary.json`: issue-class counts
- `category_summary.json`: per-category summaries
- `cohort_comparison.json`: curated vs broad cohort comparison
- `correlations.json`: key Spearman associations
- `findings_concentration.json`: concentration / Pareto metrics
- `surface_summary.json`: instruction-surface prevalence
- `surface_effects.json`: surface-presence effects
- `activity_covariates.json`: maintenance/popularity coverage summary
- `activity_effects.json`: stale-bucket summary

## Omitted on purpose

This public bundle does **not** include raw repo-path-heavy sidecars such as
`repo_metrics.json`, `sessions-of-concern.json`, `files-of-concern.json`, or
full corpus scan JSON. Those artifacts still contain local filesystem paths or
repo-granular data better handled in internal/research bundles.
