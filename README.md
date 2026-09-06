# Project Template

Starter template for new projects: React Native (Expo) mobile app,
Next.js web app, Azure Functions backend, deployed via GitHub Actions.

See `CLAUDE.md` for the full stack decisions and conventions Claude
follows automatically in this repo.

## First-time setup (once per new project created from this template)

Run these from the repo root after cloning:

```bash
# Mobile app (Expo)
npx create-expo-app@latest apps/mobile --template blank-typescript

# Web app (Next.js)
npx create-next-app@latest apps/web --typescript

# Backend (Azure Functions)
cd backend/azure-functions
npm install -g azure-functions-core-tools@4 --unsafe-perm true
func init . --typescript
cd ../..
```

Then just tell Claude Code what you want to build — it will follow
`CLAUDE.md` automatically.

## One-time machine setup (do this once, not per project)

```bash
# GitHub CLI
gh auth login

# Azure CLI
az login

# Node version manager (recommended: mise)
curl https://mise.run | sh
```

## Folder structure

```
apps/mobile/            Expo app
apps/web/                Next.js app
backend/azure-functions/ Azure Functions app
.github/workflows/       CI/CD
CLAUDE.md                Conventions Claude reads automatically
```
