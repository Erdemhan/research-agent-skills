# Research skills

Agent skills for a doctoral study on UAV-assisted IoT data collection security.
Each folder holds one skill as a `SKILL.md` with YAML frontmatter.

Imported into Claude Science through **Settings > Skills > Add skill > Import
from GitHub**. Imports do not update themselves: after pushing a change here,
open **Settings > Skills > Imported**, use the three-dot menu beside this
repository and select **Check for updates…**.

The same folders are used by Claude Code from `~/.claude/skills/`.

## Written for this project

Twelve skills below came out of auditing a finished study and finding what no
general skill covered. Each one encodes a failure that reached a late draft.

### Scientific and theoretical

| skill | what it catches |
| --- | --- |
| `parameter-provenance-trace` | a constant restated wrongly at an intermediate citation, a value attached to the wrong symbol, a measurement band silently changed |
| `link-budget-verification` | a derived RF quantity that no longer reproduces from its constants, dB and linear scales mixed, a slant range reported as a ground range |
| `equation-code-correspondence` | a published equation the code does not implement, a guard present in one and absent from the other |
| `symbol-table-audit` | one symbol with two meanings, one quantity with two symbols, a subscript colliding with a standalone symbol |
| `metric-definition-audit` | a metric conditioned on an event the compared methods reach at different rates |
| `corpus-terminology-check` | a term asserted to be standard without counting it in the sources the project holds |

### Manuscript

| skill | what it catches |
| --- | --- |
| `turkish-latex-consistency` | Turkish case suffixes broken by renumbering, cross-reference drift after floats move |
| `citation-pdf-verification` | a citation pointing at a real paper by the right authors that does not contain the claim |
| `multilingual-manuscript-sync` | a correction carried to two documents out of three |
| `latex-float-layout` | float-only pages, half-empty columns, a figure drawn too wide to be legible |
| `terminology-consistency-audit` | a heading asserting the opposite of its section, spelling and hyphenation splits |
| `paper-asset-extraction` | figures and tables pulled from the real source rather than redrawn or screenshotted |

## General

| Group | Skills |
| --- | --- |
| Integrity and rigour | `academic-integrity`, `empirical-rigor`, `fair-comparison`, `statistical-validity`, `paper-to-reproducible-code`, `research-debug` |
| Study design | `hypothesis-framing`, `simulation-experiment-design`, `rl-environment-design`, `rl-reward-shaping`, `rl-marl-design` |
| Implementation | `ray-rllib`, `pytorch-training`, `rl-paper-implementation`, `rl-offline-dataset`, `function-spec-writer`, `rl-security-review` |
| Runs and analysis | `rl-experiment-tracking`, `wandb-mlflow-api`, `result-analysis-pipeline`, `git-research` |
| Writing and release | `paper-structure`, `latex-bibtex-manager`, `academic-peer-review`, `advisor-report-generator`, `replication-package`, `dataset-documentation` |

## What is deliberately not here

- **`literature-review`** — Claude Science ships a literature review skill bound
  to its own Literature Graph connector. Two skills under the same name compete
  for the same trigger, so the local one is left out.
- **`git-engineering`** — general Git mechanics. `git-research` is kept instead,
  because experiment commit tags and keeping large artifacts out of history
  still apply.

## Caveats

- `function-spec-writer` was written for a Claude Code setup with a separate
  implementation agent. The specification format stands on its own, but the
  delegation step it describes has no counterpart here.
- `git-research` and `replication-package` assume a Git checkout. They do not
  apply to work done outside one.
- `rl-offline-dataset`, `wandb-mlflow-api` and `rl-experiment-tracking` cover
  tooling this project does not currently use. They are kept for later work.
