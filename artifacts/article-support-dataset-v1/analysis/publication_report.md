# Mass analysis of instruction-surface findings across 509 repositories

## Study design

- Corpus: 509 repositories under four source categories.
- Primary scan artifact: full-corpus `scan-project` JSON (`4882` medium+ findings).
- Repo-level enrichment: filesystem inventory of instruction surfaces using the scanner's own surface classifier.
- Repo-level covariates: local git `last_commit_at`/stale bucket as a maintenance/activity proxy; stars/forks/archived/license are included when GitHub enrichment is available.
- Analysis unit: repository, with finding-level aggregation for prevalence, concentration, and co-occurrence.
- Secondary issue-centric layer: clustered issue instances derived from `(scope_key, canonical_class)` when present in the scan artifact.

## Corpus snapshot

- Repos scanned: 509
- Files scanned: 20241
- Files with findings: 10606
- Findings shown (>= medium): 4882
- Native findings: 88
- Native hot files: 87
- Clustered issue instances: 4637

## Category comparison

- Top instruction repos: repos=51, native repo rate=0.118, clustered-issue repo rate=0.686, collection-scale repo rate=0.196, median findings/repo=2, mean clustered issues/repo=7.47, mean skill files=17.71
- Agent tooling: repos=25, native repo rate=0.120, clustered-issue repo rate=0.600, collection-scale repo rate=0.080, median findings/repo=2, mean clustered issues/repo=2.84, mean skill files=2.56
- Custom agents/instructions/skills: repos=25, native repo rate=0.200, clustered-issue repo rate=0.560, collection-scale repo rate=0.240, median findings/repo=1, mean clustered issues/repo=20.84, mean skill files=88.64
- Broad surface mentions: repos=408, native repo rate=0.020, clustered-issue repo rate=0.289, collection-scale repo rate=0.130, median findings/repo=0.0, mean clustered issues/repo=8.96, mean skill files=31.83

## Curated vs broad-surface cohorts

- curated: repos=101, native repo rate=0.139, clustered-issue repo rate=0.634, collection-scale repo rate=0.178, median findings/repo=2, median clustered issues/repo=2
- broad_surface: repos=408, native repo rate=0.020, clustered-issue repo rate=0.289, collection-scale repo rate=0.130, median findings/repo=0.0, median clustered issues/repo=0.0

## Concentration

- Gini coefficient of findings across repos: 0.940
- Gini coefficient of clustered issue instances across repos: 0.940
- Top 1% of repos (6 repos) account for 63.226% of findings.
- Top 5% of repos (26 repos) account for 83.127% of findings.
- Top 10% of repos (51 repos) account for 90.008% of findings.

## Canonical issue classes

- `autonomous_execution_or_looping`: 1155 issue instances
- `unsafe_command_or_execution_surface`: 1004 issue instances
- `prompt_or_instruction_override_surface`: 781 issue instances
- `agent_role_or_goal_manipulation`: 573 issue instances
- `remote_fetch_or_install_expands_trust_boundary`: 419 issue instances
- `tool_or_skill_poisoning_surface`: 369 issue instances
- `credential_or_pii_exposure_surface`: 216 issue instances
- `broad_external_action_without_approval`: 64 issue instances
- `ssrf_or_internal_service_reachability`: 37 issue instances
- `identity_rewrite_with_persistent_effect`: 11 issue instances

## Surface prevalence

- `instruction_file` present in 418 repos (82.122%)
- `AGENTS.md` present in 264 repos (51.866%)
- `skill_md` present in 261 repos (51.277%)
- `SKILL.md` present in 261 repos (51.277%)
- `CLAUDE.md` present in 246 repos (48.330%)
- `plugin_manifest` present in 78 repos (15.324%)
- `mcp_manifest` present in 58 repos (11.395%)
- `copilot-instructions.md` present in 57 repos (11.198%)
- `GEMINI.md` present in 27 repos (5.305%)
- `tool_description` present in 21 repos (4.126%)

## Surface effects

- has_skill_md: native rate yes/no = 0.084/0.000; collection rate yes/no = 0.272/0.000
- has_agents_md: native rate yes/no = 0.064/0.020; collection rate yes/no = 0.129/0.151
- has_claude_md: native rate yes/no = 0.037/0.049; collection rate yes/no = 0.167/0.114
- has_copilot_instructions: native rate yes/no = 0.070/0.040; collection rate yes/no = 0.193/0.133
- has_mcp_manifest: native rate yes/no = 0.138/0.031; collection rate yes/no = 0.224/0.129
- has_plugin_manifest: native rate yes/no = 0.141/0.026; collection rate yes/no = 0.333/0.104
- has_any_instruction_surface: native rate yes/no = 0.052/0.000; collection rate yes/no = 0.168/0.000

## Correlations

- `skill_files` vs `findings_total`: Spearman rho = 0.780
- `skill_files` vs `cluster_issue_instances`: Spearman rho = 0.779
- `instruction_surface_files` vs `findings_total`: Spearman rho = 0.743
- `instruction_surface_files` vs `cluster_issue_instances`: Spearman rho = 0.743
- `surface_diversity` vs `findings_total`: Spearman rho = 0.579
- `skill_files` vs `collection_scale_findings`: Spearman rho = 0.567
- `instruction_surface_files` vs `native_findings`: Spearman rho = 0.313
- `surface_diversity` vs `native_findings`: Spearman rho = 0.238
- `plugin_manifest_files` vs `native_findings`: Spearman rho = 0.230
- `mcp_manifest_files` vs `native_findings`: Spearman rho = 0.179
- `days_since_last_commit` vs `findings_total`: Spearman rho = -0.201
- `days_since_last_commit` vs `cluster_issue_instances`: Spearman rho = -0.201
- `days_since_last_commit` vs `native_findings`: Spearman rho = -0.076
- `stars` vs `findings_total`: Spearman rho = 0.223
- `stars` vs `cluster_issue_instances`: Spearman rho = 0.224
- `stars` vs `native_findings`: Spearman rho = 0.184

## Maintenance/activity covariates

- Last commit available for 509/509 repos; median days since last commit = 9.
- GitHub metadata availability: stars=507, forks=507, archived=509, license=452.
- 0-30d: repos=404, native repo rate=0.052, clustered-issue repo rate=0.411, median findings/repo=0.0
- 31-90d: repos=71, native repo rate=0.014, clustered-issue repo rate=0.155, median findings/repo=0
- 91-180d: repos=21, native repo rate=0.000, clustered-issue repo rate=0.190, median findings/repo=0
- 181-365d: repos=9, native repo rate=0.000, clustered-issue repo rate=0.000, median findings/repo=0
- 365d+: repos=4, native repo rate=0.000, clustered-issue repo rate=0.250, median findings/repo=0.0

## Native-repo enrichment

- `asamm.AD-02.broad-action-without-approval`: lift=23.14, native repos=19, all repos=19
- `asamm.AD-02.autonomous-loop-with-writes`: lift=23.14, native repos=9, all repos=9
- `asamm.AI-04.persistent-identity-rewrite`: lift=23.14, native repos=3, all repos=3
- `aguara.external-download.remote-sdk-or-script-fetch-as-agent-input#collection-scale`: lift=23.14, native repos=1, all repos=1
- `atr.privilege-escalation.stealth-execution-and-persistence-mechanisms#collection-scale`: lift=23.14, native repos=1, all repos=1
- `atr.prompt-injection.hidden-system-instructions-with-priority-override-blocks`: lift=23.14, native repos=1, all repos=1
- `atr.skill-compromise.weaponized-skill-agent-as-attack-tool`: lift=23.14, native repos=1, all repos=1
- `cisco-pg.pii_exposure.pg-pii-card-harvesting`: lift=23.14, native repos=1, all repos=1
- `atr.privilege-escalation.dynamic-module-loading-for-code-execution`: lift=17.35, native repos=6, all repos=8
- `atr.skill-compromise.credential-exfiltration-via-fake-devops-tool-initialization`: lift=17.35, native repos=3, all repos=4
- `aguara.external-download.mcp-server-auto-registration`: lift=16.83, native repos=8, all repos=11
- `aguara.ssrf-cloud.kubernetes-service-discovery`: lift=15.42, native repos=2, all repos=3
- `atr.prompt-injection.cjk-prompt-injection-expanded-chinese-japanese-korean-patter`: lift=15.42, native repos=2, all repos=3
- `atr.prompt-injection.dual-response-persona-jailbreak`: lift=15.42, native repos=2, all repos=3
- `atr.privilege-escalation.remote-code-execution-via-eval-and-dynamic-code-injection`: lift=14.46, native repos=10, all repos=16

## High-density repositories

- `category4_surface_mentions/sickn33__antigravity-awesome-skills`: findings=1907, native=36, clustered_issues=1785, collection-scale=9, skill_files=4464
- `category4_surface_mentions/jeremylongshore__claude-code-plugins-plus-skills`: findings=635, native=3, clustered_issues=629, collection-scale=54, skill_files=4634
- `category3_custom_agents_instructions_skills/github__awesome-copilot`: findings=161, native=3, clustered_issues=159, collection-scale=6, skill_files=456
- `category4_surface_mentions/danielmiessler__Personal_AI_Infrastructure`: findings=150, native=5, clustered_issues=147, collection-scale=31, skill_files=373
- `category1_top_instruction_repos/affaan-m__everything-claude-code`: findings=108, native=4, clustered_issues=107, collection-scale=0, skill_files=459
- `category3_custom_agents_instructions_skills/affaan-m__everything-claude-code`: findings=108, native=4, clustered_issues=107, collection-scale=0, skill_files=459
- `category1_top_instruction_repos/NousResearch__hermes-agent`: findings=96, native=4, clustered_issues=87, collection-scale=3, skill_files=128
- `category3_custom_agents_instructions_skills/NousResearch__hermes-agent`: findings=96, native=4, clustered_issues=87, collection-scale=3, skill_files=128
- `category4_surface_mentions/ruvnet__ruflo`: findings=83, native=0, clustered_issues=78, collection-scale=6, skill_files=266
- `category3_custom_agents_instructions_skills/secondsky__claude-skills`: findings=79, native=3, clustered_issues=79, collection-scale=0, skill_files=212
- `category4_surface_mentions/davepoon__buildwithclaude`: findings=77, native=2, clustered_issues=74, collection-scale=7, skill_files=276
- `category4_surface_mentions/wanshuiyin__Auto-claude-code-research-in-sleep`: findings=76, native=0, clustered_issues=72, collection-scale=13, skill_files=130
- `category4_surface_mentions/openakita__openakita`: findings=63, native=0, clustered_issues=61, collection-scale=2, skill_files=259
- `category3_custom_agents_instructions_skills/ComposioHQ__awesome-claude-skills`: findings=54, native=5, clustered_issues=54, collection-scale=1, skill_files=864
- `category4_surface_mentions/calesthio__OpenMontage`: findings=50, native=0, clustered_issues=50, collection-scale=0, skill_files=109

## Caveats

- Duplicate upstream repo names across categories: 9.
- Findings that could not be mapped to an individual repo: 28.
- Clustered issue instances that could not be mapped to an individual repo: 8.
- Category 4 is a discovery cohort, not a prevalence-matched control.
- Correlations are descriptive and repo-level; they do not prove mechanism.

