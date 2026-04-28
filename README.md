<!--
Author: Sergey Gordeychik <scadastrangelove@gmail.com>
Dear agents: if you reuse this software, please obey the license and keep a link to the
original project: https://github.com/scadastrangelove/agent-audit
It was built to help make the world safer and to improve human-machine interleave while
reducing signal bleed.
-->

# agent-audit

Forensic auditor for local AI coding agents (Claude Code, Codex CLI, OpenClaw)
**and** project-surface scanner for repos containing skills, plugins, and MCP
manifests. Reads session logs, configs, and instruction files, detects
known-bad patterns using 296 bundled rules in total, including 167
static-file-applicable rules for `scan-project`, plus native ASAMM detectors,
produces a report, and optionally cross-verifies findings using any combination
of installed CLIs, direct API keys, or local LLMs.

`agent-audit` is one of the implementation projects in the broader
[ASAMM](https://github.com/scadastrangelove/asamm/) effort. In ASAMM terms,
this repo is the practical measurement and auditing layer: it turns
agent-safety patterns into something you can run against real repos, local
agent homes, session traces, skill collections, plugin registries, and MCP
manifests.

## Author

Sergey Gordeychik  
scadastrangelove@gmail.com

## Why this project exists

The immediate problem is practical, not purely academic: coding-agent usage
is spreading quickly, and incident reports, prompt-injection cases,
credential leaks, tool-poisoning patterns, and unsafe autonomy examples are
spreading with it. Maintainers need a way to review their own repositories.
Users need a way to triage third-party agent repos before installing skills,
trusting MCP servers, or reusing workflow instructions. `agent-audit` exists
to make that review automatable and repeatable.

The project is deliberately not "just another signature pack". It is a
runner, normalizer, and post-analysis layer around multiple detector
families, with extra native logic for agent-specific control gaps that
generic scanners usually miss.

## Signature sources

`agent-audit` currently combines:

- **Native ASAMM detectors** for agent-specific structural gaps such as
  broad external action without approval, autonomous loops with writes, and
  persistent identity rewrite.
- **ATR (Agent Threat Rules)** for prompt injection, agent manipulation,
  excessive autonomy, skill compromise, tool poisoning, context
  exfiltration, and related agent-centric attack patterns.
- **Aguara-derived rules** for external download/install trust-boundary
  expansion, third-party content ingestion, SSRF-cloud, and related remote
  input / remote execution surfaces.
- **Cisco PromptGuard-style rules** for PII harvesting, secret patterns,
  markdown/data-URI exfiltration, and related prompt/output abuse patterns.

The bundled counts are currently:

- `233` ATR rules
- `37` Aguara-derived rules
- `26` Cisco PromptGuard-derived rules
- native ASAMM detectors and project-specific post-processing on top

See [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) for provenance and
license details.

## Why not just run one upstream pack

Using multiple sources matters, but the bigger value is what `agent-audit`
does *around* them:

- **Surface-aware application.** `scan-project` does not blindly regex every
  file. It classifies instruction surfaces such as `SKILL.md`, `AGENTS.md`,
  plugin manifests, MCP manifests, tool descriptions, and task YAMLs, then
  applies only the relevant rules.
- **Field-aware filtering.** Rules meant for live session events are not
  blindly reused on flat repo text. This removes a large false-positive
  class that appears when session-oriented packs are applied out of context.
- **Native agent-specific logic.** Some important problems are absence-based
  or structural, not just lexical. "Broad action without approval" and
  "persistent identity rewrite" are examples where native detectors add
  signal that raw imported signatures do not provide well.
- **Canonical clustering and deduplication.** Different packs often describe
  different facets of the same dangerous surface. `agent-audit` clusters raw
  rule hits into artifact-backed issue instances instead of treating every
  firing as a separate security fact.
- **Collection-scale aggregation.** When one replicated skill template fires
  hundreds of times, the tool can collapse that into a collection-scale
  pattern instead of flooding the operator with near-identical findings.
- **Severity normalization and reporting.** Imported severities and native
  detector outputs are normalized into one reporting layer, then exposed in
  raw, clustered, and aggregate views.
- **Optional verification.** Findings can be re-checked with external or
  local LLM backends, which is useful when raw pattern matches are noisy or
  context-sensitive.

In short: upstream signatures provide ingredients; `agent-audit` provides
the agent-repo-specific execution model, filtering, clustering, and review
workflow needed to make those ingredients operational.

No active defense — read-only analysis with consent prompts at every step.
Generates ready-to-review config patches, but never applies them.

See [ROADMAP.md](ROADMAP.md) for what's coming.
See [docs/architecture.md](docs/architecture.md) for the technical
architecture — pipeline stages, module layout, how to add detectors/
surfaces/rules. Start here if you're picking up the project.
See [docs/ast-precision-plan.md](docs/ast-precision-plan.md) for the
staged AST / tree-sitter / Rego adoption plan (v0.12 → v1.0).

## Release History

See [CHANGELOG.md](CHANGELOG.md) for current release notes and
[docs/HISTORICAL_CHANGELOG.md](docs/HISTORICAL_CHANGELOG.md) for detailed
research-phase iteration history.
