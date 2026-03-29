---
name: create-pr
description: Create pull requests following conventions. Use when opening PRs, writing PR descriptions, or preparing changes for review. Determines target repo (aws-amplify upstream or nxsflow) and follows PR template.
---

# Create Pull Request

**Requires**: GitHub CLI (`gh`) authenticated.

## Step 1: Determine PR Target

```bash
git branch --show-current
git merge-base --is-ancestor upstream HEAD && echo "Based on upstream" || echo "Based on main"
```

| Based on | PR target |
|----------|-----------|
| `upstream` | `aws-amplify/amplify-data` main |
| `main` | `nxsflow/amplify-data` main |

**For upstream PRs**: Verify no nxsflow-specific content (CLAUDE.md, .claude/).

## Step 2: Write the PR Description

This repo's PR template has these sections:
- **Problem** — issue number and description
- **Changes** — summary with critical parts highlighted
- **Validation** — how changes were validated (unit/integration/E2E)

## Step 3: Create the PR

### For nxsflow PRs:

```bash
gh pr create --title "<type>(<scope>): <description>" --body "$(cat <<'EOF'
## Problem
...

## Changes
...

## Validation
...
EOF
)"
```

### For upstream PRs:

```bash
git push origin <branch-name>
gh pr create --repo aws-amplify/amplify-data \
  --title "<type>(<scope>): <description>" \
  --body "$(cat <<'EOF'
## Problem
...

## Changes
...

## Validation
...
EOF
)"
```
