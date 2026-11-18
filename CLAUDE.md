# Project Conventions

This file is read automatically by Claude Code at the start of every session
in this repo. It exists so every project built from this template behaves
the same way without re-explaining the setup each time.

## Owner context

- Owner is not a professional developer. Claude should write nearly all
  code, explain decisions in plain language, and flag anything that needs
  a manual step (accounts, payments, irreversible actions) rather than
  assuming it can proceed.
- **Claude does what it can do itself, and does not ask the owner to do it.**
  Examples: taking a screenshot of the phone (`adb exec-out screencap`),
  opening the app and tapping through to a screen (`adb shell input`),
  installing a build, running commands, reading logs, checking a web page.
  The owner is only asked for what Claude cannot do: logins, passwords,
  payments, accepting terms, physical steps (e.g. plugging in the phone),
  and decisions.
- **Reply in the owner's language** (the language the owner writes in,
  usually Danish).
- **Agree before changing anything.** Claude investigates and proposes
  freely, but does not start a change (code, docs, designs) until the
  specification is clear and the owner has said an explicit yes. A
  clarification or a partial answer is not a yes. When something needs
  specifying, Claude asks the open questions **one at a time**.
- **No hacks.** Never change user data directly in the database or in
  Azure, and never work around a rule with a one-off trick. Build the
  proper feature through the normal branch → pull request → approval →
  deploy flow. If a request can't be met within the rules, say so and
  offer options.
- **Don't work around blocks.** If a safety check or permission prompt
  stops an action, tell the owner what was blocked and why, instead of
  trying the same thing another way.
- Prefer working, simple solutions over clever ones.
- **New technologies need the owner's explicit approval before they are
  added.** That means any library or package (also ones from an
  already-used framework, such as `expo-*` modules), framework, language,
  database, cloud service or Azure resource type, or tool that isn't
  already named in this file's stack or in the approved specification.
  Claude first explains in plain language what it is, why it's needed,
  the simpler alternatives and any cost, then waits for a clear yes.
  Several can be approved in one go. A newer version of something
  already used is not a new technology.

## Owner's machine and tools

Claude usually runs in the Claude desktop app on the owner's PC, with
the terminal, the browser and (over USB) the phone available. When it
runs in the cloud instead, it cannot see the PC, and the owner runs the
steps. Either way, this is the setup:

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
- **No files on the computer.** Never save anything in the owner's
  Documents folder or elsewhere on the PC, and never refer to local
  paths in documents. Temporary files go in Claude's scratchpad and are
  deleted after use. Documents (legal, operations, business) go in the
  project's wiki (Azure DevOps), or Claude asks where they should go.
- **Privacy:** Claude does not read individual users' personal data in
  the database. Use counts and aggregates, or ask the owner.

## Stack (default for every project from this template)

- **Language:** TypeScript everywhere (mobile, web, backend). Avoid
  introducing other languages unless the task genuinely requires it
  (state the reason if so).
- **Mobile:** React Native via Expo (`apps/mobile`)
- **Web:** Next.js (`apps/web`)
- **Backend:** Azure Functions, Node.js/TypeScript (`backend/azure-functions`)
- **Database:** Azure SQL Database by default — owner prefers SQL and
  Microsoft products. Experience (TroutTracer, Oct 2026): the serverless
  tier's free allowance ran out within weeks, and every app start woke
  the database (about 0.5 kr per wake-up and a ~45 s delay). **Basic
  (5 DTU, about 32 kr/month)** was cheaper and faster for a small app.
  Choose the tier with the owner. Store files such as photos in Azure
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
- **Push, build and deploy only when the owner says so.** Commit locally
  in between. App builds and deploys are workflows started by hand
  (`workflow_dispatch`), not on every push: GitHub's 2,000 free minutes
  a month run out quickly with app builds.
- **Azure billing:** when a project is set up, check that the Azure
  payment card has not expired and that a budget alert emails the owner.
  An expired card left an invoice unpaid and Azure switched off every
  service (TroutTracer, Nov 2026).
- **No messages to users before launch.** While the app is in a closed
  test, don't propose or send news to users about changes.

## Requirements and designs

- Requirements live in `docs/requirements/` (e.g. `screens.md`); designs
  live in the Figma file linked at the top of the requirements document.
- Whenever a decision or change affects a screen, update Figma in the
  same step as the docs, link the changed frame from the docs, and note
  "Figma updated" in the open-questions table and change log.
- **Every change updates everything it touches, in the same step:** the
  code, the requirements and their change log (`docs/requirements/`),
  Figma when a screen changes, the test plan (`docs/testing/uat-plan.md`),
  the backlog (`docs/requirements/future-changes.md`) and the UAT
  checklist the owner tests from.

## What "done" looks like for a new feature

1. Code written and explained in plain language (what it does, why this
   approach).
2. Runs locally without errors.
3. Committed with a clear message.
4. If it touches Azure resources or deployment, confirmed with the owner
   first.

## Pull requests and merging

- Claude works on a `feature/...` or `fix/...` branch, opens a pull
  request into the default branch and turns on Auto-fix for it in the
  Claude app, so CI failures, merge conflicts and review comments come
  back to Claude.
- **Claude asks the owner before merging.** When every CI check passes
  and no review comment is waiting, Claude says the pull request is
  ready and merges (squash merge, branch deleted) only after the owner's
  yes. Merging never builds or deploys anything: those workflows only
  start by hand.
- Claude does not merge when a check fails, a review comment is
  unanswered, or the owner has said to wait.
