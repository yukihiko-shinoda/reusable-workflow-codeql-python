# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository provides a reusable GitHub Actions workflow (`workflow_call`) for CodeQL security analysis. Consuming repositories reference it via `uses: <owner>/reusable-workflow-codeql-python/.github/workflows/workflow.yml@<ref>`.

## Repository Structure

- [.github/workflows/workflow.yml](.github/workflows/workflow.yml) — the single reusable workflow. Runs CodeQL on `actions` and `python` language matrices.
- [.github/dependabot.yml](.github/dependabot.yml) — weekly Dependabot updates for GitHub Actions pinned versions.

## How Consumers Use This Workflow

Consuming repositories reference the workflow with `uses:` and must grant the required permissions themselves:

```yaml
jobs:
  codeql:
    uses: yukihiko-shinoda/reusable-workflow-codeql-python/.github/workflows/workflow.yml@main
    permissions:
      security-events: write
      packages: read
      actions: read
      contents: read
```

The workflow grants these same permissions internally; callers must mirror them because `workflow_call` does not inherit the caller's permissions automatically.

## Key Constraints

- The workflow targets two languages: `actions` (workflow files) and `python`, both with `build-mode: none`.
- Action versions are pinned (`actions/checkout@v6`, `github/codeql-action/init@v4`, `github/codeql-action/analyze@v4`). Dependabot keeps these current — do not manually bump unless there is a specific reason.
- The workflow exposes no inputs or secrets; callers get the default CodeQL configuration. If custom queries or additional languages are needed, they must be added to the matrix `include` list.
