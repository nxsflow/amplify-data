# Amplify Data (nxsflow fork)

Fork of `aws-amplify/amplify-data`. See skill `branching-strategy` for the fork workflow.

## Project Overview

Amplify Data is a monorepo containing the TypeScript schema builder (`@aws-amplify/data-schema`) and its type definitions (`@aws-amplify/data-schema-types`). It provides the `a.*` API that developers use to define data models, auth rules, AI conversations, and generations in Amplify Gen 2.

## Monorepo Setup

- **Package manager**: Yarn 4 (Berry) with workspaces
- **Orchestration**: Turborepo (`turbo.json`)
- **Build**: Rollup → `dist/` output (ESM)
- **TypeScript**: ES2022, module `esnext`, bundler resolution, strict mode
- **Changesets**: required for all published package changes (pre-push hook enforces)

## Key Commands

```bash
yarn                         # Install dependencies
yarn build                   # Build all packages (Turbo)
yarn test                    # Run all tests (Turbo)
yarn lint                    # ESLint (Turbo)
yarn check                   # build + test + lint + check:api + check:type-perf
yarn check:api               # API surface validation (api-extractor)
yarn check:type-perf         # TypeScript performance benchmarks
yarn vend                    # Local npm proxy (Verdaccio) for testing
yarn e2e                     # Run E2E tests
```

## Packages

| Package | Purpose |
|---------|---------|
| `data-schema` | The `a.*` schema builder API — models, auth, AI, custom types |
| `data-schema-types` | Shared TypeScript types (`AiModel`, builder types, etc.) |
| `benches` | TypeScript type-level performance benchmarks |
| `integration-tests` | Integration tests |
| `e2e-tests/*` | E2E test suites (exports, node, sandbox, vite, webpack) |

## Key File: AI Model Registry

`packages/data-schema/src/ai/ModelType.ts` — maps friendly model names to Bedrock model IDs:

```typescript
a.ai.model('Claude 3.5 Haiku') → { resourcePath: 'anthropic.claude-3-5-haiku-20241022-v1:0' }
```

The `AiModel` type is defined in `packages/data-schema-types/src/builder/types.ts`.

## Code Style

- **Formatter**: Prettier (single quotes only — minimal config)
- **Linter**: ESLint 9 flat config (`eslint.config.mjs`) with typescript-eslint + import-x
- **No** `@ts-ignore`, no extraneous deps, no tabs
- **ESM**: `"type": "module"` — use `import`/`export`, not `require`

## Testing

- **Framework**: Jest with `ts-jest` preset
- **Config**: `jest.config.json` at root
- **Pre-push hook**: `yarn changeset status --since main` + `yarn check:api`

## Release Process

Uses **changesets** (same as amplify-backend):

```bash
yarn changeset              # Create a changeset
yarn publish:release        # Version + publish to npm
yarn publish:preid          # Publish snapshot/prerelease
```

## PR Process

- PR template: Problem / Changes / Validation sections
- CODEOWNERS: `@aws-amplify/amplify-js` general, `@aws-amplify/amplify-js-admins` for tests/scripts/package.json
- Conventional commits preferred
