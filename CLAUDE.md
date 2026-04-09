# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is the **Coding Duck srl** shared GitHub Actions repository. It publishes reusable GitHub Actions (composite actions) and reusable workflows for the organization to consume across projects.

## Repository Structure

```
shared-actions/
├── .github/
│   └── workflows/          # Reusable workflows (triggered via workflow_call)
├── docs/                   # Detailed per-action/workflow documentation
├── <action-name>/          # Each composite action lives in its own root-level directory
│   ├── action.yml          # Action definition
│   └── README.md           # Quick reference for this action
└── README.md               # Main index of all available actions/workflows
```

## Two Types of Reusable GitHub Actions Content

### 1. Composite Actions (`<action-name>/action.yml`)
- Live in a root-level directory named after the action
- Called by consumers via `uses: codingducksrl/<repo-name>/<action-name>@<version>`
- Each must have a `README.md` in its directory and a corresponding entry in `docs/`

### 2. Reusable Workflows (`.github/workflows/<workflow-name>.yml`)
- Must have `on: workflow_call:` trigger
- Called by consumers via `uses: codingducksrl/<repo-name>/.github/workflows/<workflow-name>.yml@<version>`
- Each must have a corresponding entry in `docs/`

## Documentation Rules

Every new action or reusable workflow **must** be accompanied by:

1. **Quick reference** (`<action-name>/README.md` or inline in the workflow file header): inputs, outputs, basic usage example.
2. **Detailed doc** (`docs/<action-name>.md` or `docs/<workflow-name>.md`): full parameter reference, example usage patterns, behavior notes, version history.
3. **Root `README.md` update**: add an entry to the index table with name, type (action/workflow), and a one-line description.

## Local Testing

Use [`act`](https://github.com/nektos/act) to run workflows locally:

```bash
# Run a specific workflow
act -W .github/workflows/<workflow-name>.yml

# Run with a specific event
act push -W .github/workflows/<workflow-name>.yml

# Run with custom inputs (for workflow_call)
act workflow_call -W .github/workflows/<workflow-name>.yml --input key=value
```

Validate YAML syntax before committing:
```bash
# Validate action.yml
actionlint <action-name>/action.yml

# Validate all workflow files
actionlint .github/workflows/*.yml
```

## Releases

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full release process and versioning rules.
