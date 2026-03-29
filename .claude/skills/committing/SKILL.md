---
name: committing
description: Create commits following conventions. Use when committing code changes, writing commit messages, or formatting git history. Covers conventional commits, changesets, and hooks.
---

# Committing in amplify-data

## Prerequisites

Before committing, ensure you're on a feature branch, not `main` or `upstream`.

Branch naming: `<type>/<short-description>` (e.g., `feat/add-inference-profiles`, `fix/model-type-mapping`)

## Commit Message Format

Conventional commits preferred:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Purpose |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `chore` | Maintenance, deps |
| `refactor` | Refactoring |
| `test` | Test changes |

### Scopes

Use package names: `data-schema`, `data-schema-types`, `deps`, `e2e`.

### Examples

```
feat(data-schema): add regional inference profile support to model registry

Add us.*, eu.*, ap.* prefixed model entries to supportedModelsLookup
for cross-region inference profiles.

Refs aws-amplify/amplify-backend#2989
Co-Authored-By: Claude <noreply@anthropic.com>
```

## Changesets

**Required** for all changes to published packages. The pre-push hook enforces this.

```bash
yarn changeset              # Interactive — select packages, bump type, description
```

Creates a file in `.changeset/` — commit it with your changes.

## Pre-push Hook

Husky runs on push:
1. `yarn changeset status --since main` — ensures changesets exist
2. `yarn check:api` — validates public API surface

If changeset check fails: run `yarn changeset`.
If API check fails: rebuild and check if API surface changed, update accordingly.

## Staging

- Stage specific files, not `git add -A`
- Never commit `.env`, credentials, or secrets
