# Sample Prompt — IT Head Project Initialisation in Claude Code (`gh` CLI)

> **Audience:** IT Head
> **Purpose:** A ready-to-use sample prompt that initialises a new project's **foundation entirely in Claude Code**, using the `gh` CLI for all GitHub operations and local files for documents. It implements the IT Head's portion of the [New Project Setup Guide](../new-project-setup-guide.md) (Step 3 — GitHub, Step 4 — Folder Structure) without depending on the GitHub MCP, which is not yet reliable inside Cowork.
> **Version:** 1.0 — June 2026

---

## 1. What This Sample Does

This prompt has the IT Head set up **only the project foundation** and then hand everything else off to the role that owns it. Run once in Claude Code, it will:

- Create the standard Google Drive document structure and the Project Configuration Sheet
- Create two GitHub repositories as **foundation shells** (no application source code), with branch strategy and branch protection
- Produce a ready-to-paste Cowork project instructions file
- Produce a **handoff document** that assigns all remaining setup work to its owning role

It deliberately does **not** perform work that belongs to other roles. The scope boundary is the most important part of this sample.

### Scope boundary — what the IT Head does NOT do here

| Work | Owner |
|---|---|
| Labels, milestones, GitHub Projects board, Linear team & cycles, ceremony scheduling, Fireflies auto-join, Sprint 0 issues, kickoff | **Project Manager** |
| CI/CD workflows, PR/Issue templates, staging/production, Sentry projects, Datadog dashboards | **DevOps** |
| PRD | **Product Development** |
| Architecture, ERD, API contract | **Backend / Tech Lead** |
| Roadmap | **Project Manager** |
| Figma design files | **UIX Designer** |

The IT Head never scaffolds application source code either — each engineer bootstraps the framework when they connect to the repository.

---

## 2. Why Claude Code (and `gh`) rather than Cowork MCP

All GitHub operations run through the **`gh` CLI** and `git` in the terminal, not the GitHub MCP connector. The GitHub MCP is currently unstable inside Cowork, so the entire initialisation is performed in Claude Code, where `gh` is reliable. Documents are written as local files inside the Google Drive–synced folder. No MCP connector is required to run this prompt.

---

## 3. How to Use

1. Open a terminal and `cd` into the project's Google Drive–synced documents folder (the `DOCS_DIR` below).
2. Run `claude`.
3. Confirm prerequisites: `gh auth status` (logged in, with access to the `qtrust-id` org), `gh --version`, `git --version`. If not logged in, run `gh auth login`.
4. Copy the **entire** code block under "The Prompt" as your first message. Run it once.

> The values below are a **worked sample for the HRIS project**. Adapt the variables for your own project — the process stays the same, only the values change.

### Variables (sample values — replace per project)

- `GITHUB_OWNER`: `qtrust-id` (organization)
- `PROJECT_CODE`: `HRIS`
- `WEB_REPO`: `hris-app` (Laravel 13 — REST API + Blade web admin)
- `MOBILE_REPO`: `hris-mobile` (Flutter)
- `REPO_VISIBILITY`: `private`
- `TEAM_COLLABORATOR`: `reynoandriano`
- `DOCS_DIR`: the project's Google Drive document folder (where `claude` is run)
- `DOC_LANG`: English

> The repositories are created as **foundation shells** (no application source code). Foundation files are pushed via a **temporary clone** (`mktemp -d`) that is deleted immediately afterwards — there is no permanent local clone.

---

## 4. The Prompt

> Copy the entire code block below into Claude Code.

````text
You are a Tech Lead assisting the IT Head of QTrust (PT Langgeng Sukses Abadi Tekhnologi) to INITIALISE the foundation of the HRIS project ENTIRELY in Claude Code, following "new-project-setup-guide.md". All GitHub operations MUST go through the `gh` CLI and `git` in the terminal (do NOT use the GitHub MCP). Documents are written as local files in the Google Drive folder. Your scope is ONLY the basic foundation plus a handoff document. Do NOT perform deliverables or processes owned by other roles.

WORK YOU MUST NOT DO (record it as handoff in Step 5 instead):
- Labels, milestones, GitHub Projects board, Linear team/cycles, ceremony scheduling, Fireflies auto-join, Sprint 0 issues, kickoff → Project Manager.
- CI/CD workflows, PR/Issue templates, staging/production, Sentry projects, Datadog dashboards → DevOps.
- PRD content → Product Development; architecture, ERD, API contract → Backend/Tech Lead; roadmap → PM.
- Figma design files → UIX Designer.
Do not scaffold application source code (no app/, lib/, etc.) — engineers bootstrap the framework themselves.

WORKING RULES
- All GitHub operations go through `gh`/`git`; compose and RUN the commands, do not just display them. Maintain a task list to track progress. All files and commit messages in English (Conventional Commits).
- For operations that need `gh api` (branch protection, collaborators), use the appropriate REST endpoint. If something fails due to permissions/plan, do not force it — record it as a manual step.
- Verify every state-changing action with a read command. Do not advance if a previous step failed.

PRINCIPLES FROM THE GUIDE (MANDATORY)
- "Documents → Google Drive. Code → GitHub. Never mix." Documents live in DOCS_DIR, code lives in the repos.
- Repo naming: qtrust-id/[project-code]-app → web `hris-app`, mobile `hris-mobile`.
- Branches: `main` (production, only accepts PRs from `release/*`), `develop` (integration → staging auto-deploy), `feature/[module]-[description]`, `fix/[issue-number]-[description]`, `release/*`. No direct pushes to `main`/`develop`.
- The Project Configuration Sheet is the single source of truth; save it in the DOCS_DIR root and link it from the repo READMEs.

VARIABLES
- DOCS_DIR = the current working folder (Google Drive document folder)
- Org = qtrust-id, web repo = hris-app, mobile repo = hris-mobile, visibility = private, collaborator = reynoandriano
- Foundation files are pushed via a TEMPORARY clone (`mktemp -d`) then deleted. No permanent clone.

CONTEXT (to fill the Config Sheet & READMEs — not to be written as a PRD)
- Product: QTrust internal HRIS.
- Web `hris-app`: Laravel 13 as a REST API (Sanctum) + Blade web admin + Tailwind, PostgreSQL, Redis. One backend serves both web and mobile.
- Mobile `hris-mobile`: Flutter (Android & iOS), consuming the REST API from `hris-app`.
- MVP scope (for folder naming & Config Sheet values): (1) Core Employee Data + RBAC; (2) Attendance & Leave with multi-level approval. Out of MVP: payroll, recruitment, performance.

STEP 0 — Verify the environment
Run `gh auth status`, `gh --version`, `git --version`, `gh api user --jq .login`, `gh api orgs/qtrust-id --jq .login`. If `gh` is not logged in or lacks org access, STOP and ask me to run `gh auth login`. Summarise the results.

STEP 1 — Document structure in Google Drive (Guide Step 4; write into DOCS_DIR)
Create the standard QTrust folder structure exactly as in the guide, with a short README.md in EVERY subfolder explaining its purpose + the OWNING ROLE:
  docs/{product,api,architecture,meetings,onboarding}
  designs/{wireframes,mockups,assets,design-system}
  by-team/{product,uix,pm,frontend,backend,vision,qa,devops,mobile}
  reports/{qa,sprint}
  src-link/
Then:
1. Create `HRIS-project-config.md` in the DOCS_DIR root using the Project Configuration Sheet template (Guide Section 3). Fill ONLY what the IT Head certainly knows: Project Identity (Name: QTrust HRIS, Code: HRIS, Type: New Build, Confidentiality: Confidential), Technology Stack (Laravel 13, Blade + Tailwind, PostgreSQL, Redis, Sanctum, Mobile: Flutter, AI/Vision: None), Repository (qtrust-id/hris-app & qtrust-id/hris-mobile, Private), Documentation Language: English. Mark everything else (cloud provider, deployment target, timeline, milestone dates, team names, domain, communication) as "TBD — to be completed by IT Head & Product Head". Do NOT invent values.
2. Create `src-link/GITHUB.md` — a pointer to both repos, a branch-strategy summary, and connection/clone instructions (URLs filled in after Steps 2–3).
Do NOT create PRD, architecture, ERD, API contract, or roadmap content/stubs — only subfolder READMEs that state the owner.

STEP 2 — WEB repo: qtrust-id/hris-app (foundation shell) via gh
1. `gh repo create qtrust-id/hris-app --private --description "QTrust HRIS — Laravel 13 REST API + Blade web admin" --add-readme` (ensure it is created in the organization).
2. Clone into a TEMPORARY directory (`mktemp -d`), create & commit only the FOUNDATION files, push, then delete the temporary directory:
   - `README.md` — project description, stack summary, **how to connect to the existing repo** (clone via SSH/HTTPS + `gh`/SSH key auth per `tools/github.md`) and **how an engineer bootstraps the app** (e.g. `composer create-project laravel/laravel:^13 .`, then `cp .env.example .env`, set PostgreSQL/Redis/Sanctum, `php artisan key:generate`, `php artisan migrate`, `npm install && npm run dev`), the branch strategy, a **link to the Project Configuration Sheet** & the Drive docs folder, and a "Next Steps / Handoff" section (Step 5).
   - `.gitignore` — for Laravel + Node (per Guide Step 3).
   - `CONTRIBUTING.md` — coding standards (PSR-12 / Laravel Pint, PHPStan/Larastan), Conventional Commits, branching (`feature/[module]-[description]`, `fix/[issue-number]-[description]`, `release/*`; PR into `develop`; `main` only from `release/*`), PR rules (min. 1 approval, CI green once DevOps adds CI, reference the issue e.g. `Closes #42`).
   No application source code, CI/CD, PR/Issue templates, or product docs folder in the repo.
3. `git checkout -b develop && git push -u origin develop`.
4. Repo access: `gh api -X PUT repos/qtrust-id/hris-app/collaborators/reynoandriano -f permission=maintain`.
5. Branch protection (via `gh api -X PUT repos/qtrust-id/hris-app/branches/<branch>/protection`): `main` & `develop` require pull request review (min. 1) + no force push + no deletions. Do NOT force "required status checks" yet (no CI exists) — record it as handoff so DevOps adds it after CI is created. If it fails due to permissions/plan, record it as manual.

STEP 3 — MOBILE repo: qtrust-id/hris-mobile (foundation shell) via gh
1. `gh repo create qtrust-id/hris-mobile --private --description "QTrust HRIS — Flutter mobile app (Android & iOS)" --add-readme`.
2. Clone into a TEMPORARY directory, commit the FOUNDATION files, push, then delete it:
   - `README.md` — summary, **how to connect to the repo** (clone + auth) and **how to bootstrap the app** (`flutter create .`, then `flutter pub get`, configure the API base URL per environment, `flutter run`), a link to the Config Sheet & to the API contract (`docs/api/04-api-contract.md` in Drive — to be written by Backend), and a "Next Steps / Handoff" section.
   - `.gitignore` — standard Flutter/Dart.
   - `CONTRIBUTING.md` — `flutter analyze`, `dart format`, Conventional Commits, branching & PR rules identical to the web repo.
   No application source code or CI/CD in the repo.
3. `git checkout -b develop && git push -u origin develop`.
4. `gh api -X PUT repos/qtrust-id/hris-mobile/collaborators/reynoandriano -f permission=maintain`.
5. Branch protection for `main` & `develop` (same as Step 2.5).
6. Complete `src-link/GITHUB.md` in DOCS_DIR with both repo URLs.

STEP 4 — Prepare the Cowork instructions (a ready-to-paste file; do not run anything)
Write `docs/onboarding/cowork-instructions.md` in DOCS_DIR: the fully filled-in Cowork project instructions text based on the guide template (Section 14), using the HRIS values (stack Laravel 13 API + Blade, Sanctum, PostgreSQL, Redis, Flutter, AI: None; repo URLs qtrust-id/hris-app & qtrust-id/hris-mobile; Config Sheet path; language English). Purpose: later the IT Head simply creates the Cowork project in Claude Desktop, connects this Drive folder, and pastes this text into Project Settings → Instructions. Leave no placeholders. (This only creates a file; no MCP action.)

STEP 5 — Handoff document (NOT executed by the IT Head — only listed)
Create `docs/onboarding/next-steps-by-role.md` in DOCS_DIR detailing the REMAINING setup work and its owning role, referencing the guide:
- IT Head (manual, outside Code): create the Cowork project in Claude Desktop, connect the Drive folder, paste cowork-instructions.md; add the MCP connectors each role needs later (note: the GitHub MCP is not yet stable in Cowork — GitHub work is done via `gh`/`git` in Code for now); provision tool access per the `tools/README.md` matrix (GitHub roles, Figma Editor for UIX, Linear Member for PM, Sentry/Datadog admin for DevOps, etc.); create the shared "HRIS — Team" Google Calendar and Slack channels including #hris-alerts.
- Project Manager: labels, milestones, GitHub Projects board (columns: Backlog | Sprint Backlog | In Progress | In Review | Ready for QA | Done), Linear team + cycles, schedule ceremonies + Fireflies auto-join (invite fred@fireflies.ai), Sprint 0 issues, facilitate the kickoff.
- DevOps: CI/CD workflows (lint/test/build) in both repos, add required status checks to branch protection, GitHub Environments staging & production (production: required reviewers), Sentry projects per service, Datadog dashboards, route alerts to #hris-alerts.
- Product Development: PRD (docs/product/). Backend/Tech Lead: architecture (docs/architecture/), ERD, API contract (docs/api/). PM: roadmap (by-team/pm/).
- UIX Designer: Figma workspace + 4 files (Design System, Wireframes, Hi-Fi, Mobile).
- IT Head & Product Head: complete the TBD fields in the Project Configuration Sheet.
Link this document from both repo READMEs (the "Next Steps / Handoff" section).

CLOSING — Verify & summarise
Verify: `gh repo view qtrust-id/hris-app`, `gh repo view qtrust-id/hris-mobile`, check branches (`gh api repos/.../branches`), check the DOCS_DIR structure. Then summarise: both repo links + branches, branch-protection status, the document folder structure + Config Sheet + GITHUB.md + cowork-instructions.md + next-steps-by-role.md, and the per-role handoff list.

QTrust governance note: Claude assists, humans decide. Review the foundation before handing it to each role.
````

---

*Maintained by QTrust Engineering · https://github.com/qtrust-id/ONBOARDING*
