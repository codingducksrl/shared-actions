# enforce-pr-direction

Fails a pull request check if the source → target branch combination is not in the allowed list.
Rules are defined per-call, and both `from` and `to` support regular expressions.

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `rules` | yes | — | JSON array of `{"from": "<regex>", "to": "<regex>"}` objects |
| `head-branch` | no | `github.head_ref` | Source branch of the PR |
| `base-branch` | no | `github.base_ref` | Target branch of the PR |

## Outputs

| Output | Description |
|--------|-------------|
| `matched-rule` | The matched rule in `from -> to` format, or empty string if denied |

## Usage

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  check-direction:
    runs-on: ubuntu-latest
    steps:
      - uses: codingducksrl/shared-actions/enforce-pr-direction@v1
        with:
          rules: |
            [
              {"from": ".+",        "to": "^dev$"},
              {"from": "^dev$",     "to": "^staging$"},
              {"from": "^staging$", "to": "^main$"}
            ]
```

See [`docs/enforce-pr-direction.md`](../docs/enforce-pr-direction.md) for advanced usage and regex examples.
