# enforce-pr-direction

Fails a pull request check if the PR's source → target branch combination does not match any configured rule.

Rules are supplied on every use — this action has no built-in policy. Both `from` and `to` fields are matched using Python's `re.fullmatch`, so they must match the entire branch name.

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `rules` | yes | — | JSON array of `{"from": "<regex>", "to": "<regex>"}` objects defining allowed directions |
| `head-branch` | no | `github.head_ref` | Source branch. Override only if the action is used outside a `pull_request` event |
| `base-branch` | no | `github.base_ref` | Target branch. Override only if the action is used outside a `pull_request` event |

## Outputs

| Output | Description |
|--------|-------------|
| `matched-rule` | The matched rule as `from -> to`. Empty string if the PR was denied |

## Rules format

```json
[
  {"from": "<regex>", "to": "<regex>"},
  ...
]
```

- Patterns use `re.fullmatch` — they must match the **entire** branch name.
- Use `^` / `$` anchors explicitly if needed (they are implicit with `fullmatch`, but including them makes intent clearer).
- Invalid regex patterns in a rule emit a warning and the rule is skipped.

## Examples

### Standard dev → staging → main flow

```yaml
- uses: codingducksrl/shared-actions/enforce-pr-direction@v1
  with:
    rules: |
      [
        {"from": ".+",        "to": "^dev$"},
        {"from": "^dev$",     "to": "^staging$"},
        {"from": "^staging$", "to": "^main$"}
      ]
```

### Allow feature branches to merge only into dev

```yaml
- uses: codingducksrl/shared-actions/enforce-pr-direction@v1
  with:
    rules: |
      [
        {"from": "^feature/.+", "to": "^dev$"},
        {"from": "^dev$",       "to": "^staging$"},
        {"from": "^staging$",   "to": "^main$"}
      ]
```

### Allow hotfixes to go directly to main or staging

```yaml
- uses: codingducksrl/shared-actions/enforce-pr-direction@v1
  with:
    rules: |
      [
        {"from": "^hotfix/.+",  "to": "^main$"},
        {"from": "^hotfix/.+",  "to": "^staging$"},
        {"from": "^dev$",       "to": "^staging$"},
        {"from": "^staging$",   "to": "^main$"}
      ]
```

### Use the matched-rule output in a subsequent step

```yaml
- id: direction
  uses: codingducksrl/shared-actions/enforce-pr-direction@v1
  with:
    rules: |
      [{"from": "^dev$", "to": "^staging$"}, {"from": "^staging$", "to": "^main$"}]

- run: echo "Matched rule: ${{ steps.direction.outputs.matched-rule }}"
```

## Behavior

- The action iterates rules in order and exits on the **first match** (allow-first logic).
- If no rule matches, the step fails and all configured rules are printed to the log.
- The action is a no-op outside pull request events (both `head_ref` and `base_ref` will be empty); pin it to `pull_request` triggers.

## Versioning

Pin to a major version tag for automatic patch updates:

```yaml
uses: codingducksrl/shared-actions/enforce-pr-direction@v1
```
