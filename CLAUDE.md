# Project Conventions

This file is read automatically by Claude Code at the start of every session
in this repo. It exists so every project built from this template behaves
the same way without re-explaining the setup each time.

## Owner context

- Owner is not a professional developer. Claude should write nearly all
  code, explain decisions in plain language, and flag anything that needs
  a manual step (accounts, payments, irreversible actions) rather than
  assuming it can proceed.
- Prefer working, simple solutions over clever ones. Avoid introducing a
  new library/framework/pattern without explaining the trade-off first.

## Stack (default for every project from this template)

- **Language:** TypeScript everywhere (mobile, web, backend). Avoid
  introducing other languages unless the task genuinely requires it
  (state the reason if so).
- **Mobile:** React Native via Expo (`apps/mobile`)
- **Web:** Next.js (`apps/web`)
- **Backend:** Azure Functions, Node.js/TypeScript (`backend/azure-functions`)
- **Database:** Azure Cosmos DB by default. Use Azure SQL instead only if
  the data is clearly relational/tabular.
- **Auth:** Auth.js if a project needs simple auth; Azure AD B2C only if
  the project specifically needs enterprise-grade identity.
- **CI/CD:** GitHub Actions, deploying to Azure.
- **Package manager:** npm (not yarn/pnpm), for consistency across
  projects.

## Folder structure

```
apps/
  mobile/     # Expo app
  web/        # Next.js app
backend/
  azure-functions/   # Azure Functions app
.github/workflows/   # CI/CD pipelines
```

Shared logic (types, utility functions, API client) that both mobile and
web need should live in a `packages/shared` folder — create it the first
time it's actually needed, not preemptively.

## Conventions

- Commits: short, imperative subject line (e.g. "Add login screen"), no
  period at the end.
- Branches: `feature/<short-name>`, `fix/<short-name>`.
- Environment variables/secrets: never hard-code. Use `.env.local` files
  (git-ignored) locally, and GitHub Actions secrets / Azure App Settings
  in CI and production.
- Before running any command that costs money (provisioning Azure
  resources, deploying), state what it will do and its expected cost
  tier, and wait for explicit confirmation.

## What "done" looks like for a new feature

1. Code written and explained in plain language (what it does, why this
   approach).
2. Runs locally without errors.
3. Committed with a clear message.
4. If it touches Azure resources or deployment, confirmed with the owner
   first.
