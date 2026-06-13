# reusable-workflow-codeql-python

Reusable GitHub Actions workflow for CodeQL security analysis of Python repositories.

## Advantage

Setting up CodeQL requires the same boilerplate in every repository. This workflow centralizes that configuration so you reference one source instead of maintaining copies. Dependabot in this repository keeps the pinned action versions current, so consuming repositories benefit from version bumps automatically.

## Quickstart

Add a workflow file to your repository that calls this one:

```yaml
# .github/workflows/codeql.yml
name: "CodeQL"

on:
  push:
    branches: ["main"]
  pull_request:
    # The branches below must be a subset of the branches above
    branches: ["main"]
  schedule:
    - cron: "0 0 * * 0"

permissions:
  # required for all workflows
  security-events: write
  # required to fetch internal or private CodeQL packs
  packages: read
  # only required for workflows in private repositories
  actions: read
  contents: read
jobs:
  codeql:
    uses: yukihiko-shinoda/reusable-workflow-codeql-python/.github/workflows/workflow.yml@v1
    permissions:
      security-events: write
      packages: read
      actions: read
      contents: read
```

The workflow analyzes both `python` source files and `actions` (workflow YAML files) with `build-mode: none`.
