# Contributing

## Adding a new action or workflow

1. **Composite action** — create `<action-name>/action.yml` and `<action-name>/README.md`.
2. **Reusable workflow** — create `.github/workflows/<workflow-name>.yml` with an `on: workflow_call:` trigger.
3. Add a detailed doc to `docs/<name>.md`.
4. Add a row to the index tables in the root `README.md`.

## Creating a release

Releases are tagged on `main`. Both a versioned tag and a floating major tag must be created.

```bash
# 1. Tag the release
git tag vX.Y.Z
git tag -f vX          # create or move the floating major tag

# 2. Push tags
git push origin vX.Y.Z
git push -f origin vX

# 3. Create the GitHub release
gh release create vX.Y.Z \
  --repo codingducksrl/shared-actions \
  --title "vX.Y.Z" \
  --notes "..."
```

### Versioning rules

- Follow [semantic versioning](https://semver.org): `vMAJOR.MINOR.PATCH`
  - **PATCH** — backwards-compatible bug fixes
  - **MINOR** — new inputs/outputs that are backwards-compatible
  - **MAJOR** — breaking changes (renamed/removed inputs, changed behavior)
- The floating major tag (e.g. `v1`) is always moved forward so consumers pinned to `@v1` receive patches automatically.
- Release title is the version number only (e.g. `v1.0.0`).
