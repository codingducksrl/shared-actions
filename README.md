# shared-actions

Reusable GitHub Actions and workflows for Coding Duck srl.

## Usage

### Composite Actions

```yaml
- uses: codingducksrl/shared-actions/<action-name>@v1
  with:
    input-name: value
```

### Reusable Workflows

```yaml
jobs:
  my-job:
    uses: codingducksrl/shared-actions/.github/workflows/<workflow-name>.yml@v1
    with:
      input-name: value
    secrets: inherit
```

## Available Actions

| Name | Type | Description |
|------|------|-------------|
| [`enforce-pr-direction`](enforce-pr-direction/README.md) | composite action | Fails a PR check if the source → target branch is not in the configured allow-list |

## Available Workflows

| Name | Type | Description |
|------|------|-------------|
| *(none yet)* | — | — |

## Documentation

Detailed documentation for each action and workflow is in the [`docs/`](docs/) directory.

## Contributing

1. Add composite actions as `<action-name>/action.yml` with a `README.md` in the same directory.
2. Add reusable workflows to `.github/workflows/` with `on: workflow_call:` trigger.
3. Add a detailed doc to `docs/<name>.md`.
4. Update the tables above.
