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

## Owner's machine and tools

Claude usually runs in the cloud and cannot see the owner's PC. Give
instructions for this setup:

- **Windows** with **PowerShell** (also the Terminal inside Android
  Studio: `>_` icon bottom left, or Alt+F12). Instructions must be
  Windows/PowerShell commands, not bash.
- **Git** and the **GitHub CLI (`gh`)** are installed. Don't ask the owner
  to install them again. `gh` should be logged in to the owner's GitHub
  account: check with `gh auth status`, log in with `gh auth login`.
- The PC is a **Surface Laptop 7 with an ARM chip (Snapdragon X Elite)**.
  Google's Android emulator (virtual phone) does not run on Windows ARM
  PCs ("Virtualization extension is not supported"), so app builds are
  tested on the owner's **Android phone connected with USB**. Don't suggest
  the emulator on this PC.
- **Android Studio** is installed, including adb, the Android SDK
  command-line tools (`sdkmanager`, `avdmanager`) and the Android Emulator
  package. Command-line SDK tools need Java (Android Studio's `jbr`
  folder), and in PowerShell need `--%` before package names with
  semicolons.
- The Desktop and Documents folders are synced by **OneDrive**. Clone
  projects into the user folder (`cd $HOME`, i.e. `C:\Users\<name>\<repo>`),
  not into OneDrive folders.
- Installing an app build: use `adb install -r` (keeps the app's data).
  **Never** tell the owner to uninstall an app to fix an install error –
  that deletes data stored on the phone. A common mistake is typing
  `adb` twice in the command.
- Secrets (API keys, tokens) are never pasted into the chat or shown in
  screenshots; they go into GitHub repository secrets or the cloud
  environment's settings.

## Stack (default for every project from this template)

- **Language:** TypeScript everywhere (mobile, web, backend). Avoid
  introducing other languages unless the task genuinely requires it
  (state the reason if so).
- **Mobile:** React Native via Expo (`apps/mobile`)
- **Web:** Next.js (`apps/web`)
- **Backend:** Azure Functions, Node.js/TypeScript (`backend/azure-functions`)
- **Database:** Azure SQL Database (serverless tier) by default — owner
  prefers SQL and Microsoft products. Store files such as photos in Azure
  Blob Storage, not in the database. Use Azure Cosmos DB only if the data
  is clearly document-shaped and not relational (state the reason if so).
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

## Requirements and designs

- Requirements live in `docs/requirements/` (e.g. `screens.md`); designs
  live in the Figma file linked at the top of the requirements document.
- Whenever a decision or change affects a screen, update Figma in the
  same step as the docs, link the changed frame from the docs, and note
  "Figma updated" in the open-questions table and change log.

## What "done" looks like for a new feature

1. Code written and explained in plain language (what it does, why this
   approach).
2. Runs locally without errors.
3. Committed with a clear message.
4. If it touches Azure resources or deployment, confirmed with the owner
   first.
