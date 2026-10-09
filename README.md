# Research skills

Agent skills for a doctoral study on UAV-assisted IoT data collection security.
Each folder holds one skill as a `SKILL.md` with YAML frontmatter.

Imported into Claude Science through **Settings > Skills > Add skill > Import
from GitHub**. Imports do not update themselves: after pushing a change here,
open **Settings > Skills > Imported**, use the three-dot menu beside this
repository and select **Check for updates…**.

The same folders are used by Claude Code from `~/.claude/skills/`.

## What is here

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
