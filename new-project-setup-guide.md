# New Project Setup Guide

> **Audience:** IT Head, Product Head
> **Purpose:** Step-by-step process to initiate any new QTrust project — from first conversation to Sprint 0
> **Version:** 1.0 — June 2026

---

## Table of Contents

1. [Introduction & Philosophy](#1-introduction--philosophy)
2. [Before You Start — Project Configuration Sheet](#2-before-you-start--project-configuration-sheet)
3. [Step 1 — Complete the Project Configuration Sheet](#3-step-1--complete-the-project-configuration-sheet)
4. [Step 2 — Create the Claude Cowork Project](#4-step-2--create-the-claude-cowork-project)
5. [Step 3 — Set Up the GitHub Repository](#5-step-3--set-up-the-github-repository)
6. [Step 4 — Set Up the Folder Structure (Google Drive)](#6-step-4--set-up-the-folder-structure-google-drive)
7. [Step 5 — Set Up the Design Workspace (Figma)](#7-step-5--set-up-the-design-workspace-figma)
8. [Step 6 — Provision Infrastructure](#8-step-6--provision-infrastructure)
9. [Step 7 — Onboard Your Team](#9-step-7--onboard-your-team)
10. [Step 8 — Run the Project Kickoff](#10-step-8--run-the-project-kickoff)
11. [Complete Team Workflow](#11-complete-team-workflow)
12. [Team Composition Matrix](#12-team-composition-matrix)
13. [Standard Folder Structure (Reference)](#13-standard-folder-structure-reference)
14. [Cowork Project Instructions Template](#14-cowork-project-instructions-template)
15. [Governance & Standards](#15-governance--standards)
16. [Master Setup Checklist](#16-master-setup-checklist)

---

## 1. Introduction & Philosophy

QTrust (PT Langgeng Sukses Abadi Tekhnologi) operates multiple independent digital products. Each project may use a different technology stack, involve a different subset of teams, and serve different end users. This guide standardises the setup process while allowing full technical flexibility.

> **Core Principle:** Process is consistent. Technology is flexible. Every QTrust project follows the same 8 setup steps — the specific stack, cloud provider, and team composition are chosen fresh for each project.

This guide is maintained in the QTrust Engineering ONBOARDING repository and applies to all new projects regardless of domain, scale, or complexity. Whether you are launching a new customer-facing product, an internal operations tool, or an AI-powered feature service, you follow these steps in order.

**Why a standard process matters:**

- It eliminates ambiguity at the most fragile moment — the start
- It ensures every team member has the same information on Day 1
- It prevents common failures: missing access grants, unclear tech decisions, undocumented environments
- It makes Claude (Cowork) immediately useful, because context is captured upfront rather than reconstructed repeatedly in every conversation

### Project Setup Lifecycle

| Phase | Activity | Owner | Output |
|---|---|---|---|
| 1. Initiation | Complete Project Configuration Sheet | IT Head / Product Head | Approved Config Sheet |
| 2. Cowork Setup | Create Cowork project + connect folder | IT Head | Active Cowork project |
| 3. Repo & Docs | Create GitHub repo + Google Drive structure | IT Head / DevOps | Repo + folder structure live |
| 4. Design | Create Figma workspace + initial files | IT Head / UIX Lead | Figma files linked |
| 5. Infrastructure | Provision staging environment | DevOps | Staging environment live |
| 6. Onboarding | Distribute onboarding docs, grant access | IT Head / PM | All team members ready |
| 7. Kickoff | Kickoff meeting + Sprint 0 planning | PM | Sprint 0 started |

---

## 2. Before You Start — Project Configuration Sheet

The Project Configuration Sheet is the single most important document when starting a new project. It is a structured document that captures every project-specific decision: the product name, the technology stack, the team composition, the repository URL, the cloud provider, the design links, the timeline, and the communication protocols.

> **⚠️ Important:** All 9 team onboarding guides in this repository contain placeholders like `[PROJECT_NAME]`, `[TECH_STACK]`, `[REPO_URL]`, and `[API_BASE_URL]`. The Project Configuration Sheet provides the actual values that replace these placeholders. **No team member should begin work until they have read and understood the Project Configuration Sheet.**

The Project Configuration Sheet is completed once by the IT Head and/or Product Head before any other setup step is taken. It is then distributed to every team member and linked from every primary tool (Cowork, GitHub, Google Drive).

**Distribution:** Save the completed Config Sheet as `[PROJECT_CODE]-project-config.md` in the Google Drive project root. Link to it from the Cowork project instructions and the GitHub repository README.

---

## 3. Step 1 — Complete the Project Configuration Sheet

Before creating any repository, folder, or environment, the IT Head and Product Head must sit together and complete the Project Configuration Sheet in full. Do not leave fields blank with the intention of filling them in later. Every placeholder in the team onboarding guides depends on this document.

Work through each section below. Where a field is marked `[REQUIRED]`, it must be filled before the sheet is distributed. Where a field is marked `[if applicable]`, complete it only if the project includes that capability.

Once completed, save the file as `[PROJECT_CODE]-project-config.md` and upload it to the Google Drive project root folder. Paste the direct link into the Cowork project instructions so every conversation has access to it.

---

### Template

---

#### A. Project Identity

| Field | Value / Guidance |
|---|---|
| Project Name | `[REQUIRED]` Full product name (e.g., "QTrust Portal", "SupplyChain Tracker") |
| Project Code | `[REQUIRED]` Short 3–6 character code used in repo names, folder names, and Cowork project name (e.g., PORTAL, SCT) |
| Description | `[REQUIRED]` One paragraph — what the product does and who it is for |
| Client / Stakeholder | `[REQUIRED]` Internal product or external client name |
| Project Type | `[REQUIRED]` New Build / Enhancement / Migration / Research |
| Confidentiality | `[REQUIRED]` Internal / Confidential / Public |

---

#### B. Team Composition

| Team | Involved? | Names (if known) |
|---|---|---|
| Product Development | Yes / No | |
| UIX Designer | Yes / No | |
| Project Manager | Yes / No | |
| Frontend Engineer | Yes / No | |
| Backend Engineer | Yes / No | |
| Image Vision Engineer | Yes / No | |
| QA / QC | Yes / No | |
| DevOps | Yes / No | |
| Mobile Apps Engineer | Yes / No | |

---

#### C. Technology Stack

| Layer | Technology | Notes |
|---|---|---|
| Backend Framework + Version | | e.g., Laravel 11, Django 4.2, NestJS 10 |
| Frontend Framework | | e.g., React 18, Vue 3, Next.js 14 |
| CSS Framework | | e.g., Tailwind CSS, Bootstrap 5 |
| Database | | e.g., PostgreSQL 16, MySQL 8, MongoDB 7 |
| Cache / Queue | | e.g., Redis 7, RabbitMQ, None |
| Authentication | | e.g., Laravel Sanctum, JWT, OAuth2 |
| Mobile Approach | | e.g., React Native, Flutter, Native Android, None |
| AI / Vision Framework (if any) | | e.g., PyTorch, TensorFlow, OpenCV, Roboflow, None |
| File Storage | | e.g., GCS, S3, Cloudflare R2 |

---

#### D. Repository & Infrastructure

| Field | Value |
|---|---|
| GitHub Repository URL | |
| Cloud Provider | GCP / AWS / Azure |
| Deployment Target | e.g., Cloud Run, ECS Fargate, App Service |
| CI/CD Tool | e.g., GitHub Actions |
| Container Registry | e.g., GCR, ECR, ACR |
| Environments | dev / staging / prod |
| Domain / Subdomain | e.g., [project-code].qtrust.id, api.[project-code].qtrust.id |

---

#### E. Design & Documentation

| Field | Value |
|---|---|
| Figma Workspace Link | |
| Design System | e.g., QTrust Base DS, project-specific, None yet |
| Google Drive Folder Path | e.g., QTrust Shared Drive / [PROJECT_CODE] |
| Documentation Language | English |

---

#### F. Timeline

| Field | Value |
|---|---|
| Project Start Date | YYYY-MM-DD |
| Target Launch Date | YYYY-MM-DD |
| Sprint Length | 1 week / 2 weeks |
| Key Milestones | 1. [Name] — [YYYY-MM-DD] |
| | 2. [Name] — [YYYY-MM-DD] |
| | 3. [Name] — [YYYY-MM-DD] |
| | 4. [Name] — [YYYY-MM-DD] |
| | 5. [Name] — [YYYY-MM-DD] |

---

#### G. Communication

| Field | Value |
|---|---|
| Daily Standup | Time + format (e.g., 09:00 WIB, async via Google Chat) |
| Sprint Planning | Day + time (e.g., Monday 10:00 WIB) |
| Stakeholder Updates | Frequency (e.g., weekly written summary every Friday) |
| Escalation Path | e.g., Engineer → Tech Lead → IT Head → CTO |

---

#### H. Compliance & Security

| Field | Value |
|---|---|
| Data Classification | Public / Internal / Confidential / Restricted |
| PII / Sensitive Data Handling | Describe any personal data the system processes |
| Compliance Requirements | e.g., PDPA, GDPR, ISO 27001, None |
| Security Review Required | Yes / No — if Yes, schedule before staging deployment |

---

> **✅ When done:** Save this file as `[PROJECT_CODE]-project-config.md` in the Google Drive project root. Every team member must read it on Day 1.

---

## 4. Step 2 — Create the Claude Cowork Project

The Claude Cowork project is the AI workspace for the entire team. It connects Claude to the project's Google Drive folder, giving every team member a shared context — the same stack knowledge, the same document language preference, the same repo URL — without anyone having to re-explain the project at the start of each conversation.

1. Open Claude Desktop → click **New Project**
2. Name it: `[COMPANY_CODE]-[PROJECT_CODE]` (e.g., `QT-PORTAL`, `QT-SCT`)
3. Open Project Settings → **Instructions** → paste the template from [Section 14](#14-cowork-project-instructions-template), filled in with values from the Config Sheet
4. Settings → **Workspace** → select the local Google Drive folder for this project (the folder synced via Google Drive Desktop on the IT Head's machine, and later on each team member's machine)
5. If UIX Designer is on the team: verify Figma MCP is active — test with the prompt: `"List my Figma files"`
6. Add and verify the GitHub MCP — GitHub is **not** in the one-click directory, so add its official server as a **custom connector** (`https://api.githubcopilot.com/mcp/`); see [`tools/github.md`](tools/github.md). Test with `"List issues in qtrust-id/[repo-name]"`
7. Connect the remaining MCPs relevant to this project so Claude can act across the whole tool stack (see [`tools/README.md`](tools/README.md) for the matrix of which roles need which). Verify each with a quick prompt:
   - **Linear** (sprint management): `"List open issues in the [PROJECT_CODE] Linear team"`
   - **Google Calendar** (ceremony scheduling): `"What meetings do I have this week?"`
   - **Fireflies** (meeting transcripts): `"Fetch my recent Fireflies meetings"`
   - **Sentry** (error tracking, once projects exist): `"List open issues in the [project-code]-backend Sentry project"`
   - **Datadog** (observability, once provisioned): `"List all active Datadog monitors and their status"`
8. Add all team members to the Cowork project; confirm they accept the invitation
9. Post the Config Sheet file path in the project instructions so every conversation has easy reference

> **💡 Tip:** Good project instructions mean Claude already knows the stack, repo URL, and document language before the first message — eliminating repeated context-setting across every team conversation. Invest 10 minutes in the instructions and save hours across the project.

---

## 5. Step 3 — Set Up the GitHub Repository

### Repository Creation

- Create under the **`qtrust-id`** GitHub organisation
- Naming convention: `qtrust-id/[project-code]-app` (e.g., `qtrust-id/portal-app`, `qtrust-id/sct-app`)
- Visibility: **Private**
- Initialise with `README.md` and the appropriate `.gitignore` for the backend framework chosen in the Project Config Sheet
- Add a repository description matching the one-line product description from the Config Sheet

### Branch Strategy

| Branch | Purpose | Rules |
|---|---|---|
| `main` | Production — always deployable | No direct push. Merge only via PR from `release/*` |
| `develop` | Integration branch | All features merge here. Auto-deploys to the **staging** environment. |
| `feature/*` | New features | Branch from `develop`. Naming: `feature/[module]-[description]` |
| `fix/*` | Bug fixes | Branch from `develop` (or `main` for hotfixes). Naming: `fix/[issue-number]-[description]` |
| `release/*` | Release candidate | Created from `develop` → merged to `main` when stable |

### GitHub Setup Checklist

- [ ] Repository created under `qtrust-id` organisation (Private)
- [ ] `develop` branch created from `main`
- [ ] Branch protection on `main`: require PR, require CI pass, no direct push, no force push
- [ ] Branch protection on `develop`: require PR, require CI pass
- [ ] Labels created: `module:*`, `type:bug`, `type:feature`, `type:task`, `priority:high`, `priority:medium`, `priority:low`, `team:frontend`, `team:backend`, `team:mobile`, `team:devops`, `team:qa`, `status:blocked`
- [ ] Milestones created: one per sprint (Sprint 1, Sprint 2, …) + one for UAT + one for Launch
- [ ] GitHub Projects Kanban board created with columns: **Backlog | Sprint Backlog | In Progress | In Review | Ready for QA | Done**
- [ ] All team members added with appropriate roles (Maintainer for PM/IT Head, Write for engineers, Read for stakeholders)
- [ ] README updated with project description, stack summary, and link to Config Sheet

---

## 6. Step 4 — Set Up the Folder Structure (Google Drive)

Google Drive is for **documents**. GitHub is for **code**. Never mix them.

> **⚠️ Rule:** Never store source code in Google Drive. Never store documentation, credentials, or secrets in GitHub. **Documents → Google Drive. Code → GitHub.**

### Creating and Sharing the Project Folder

1. In Google Drive (signed in to `@qtrust.id`), create a folder named `[PROJECT_CODE]` inside the QTrust Shared Drive
2. Share with all team members — **Editor** access
3. Each team member syncs it locally via Google Drive Desktop (available at drive.google.com/drive/download)
4. Each person connects their local synced path to Claude Desktop as the workspace folder for the Cowork project — this ensures Claude can read and write documents directly without file uploads

### Standard Folder Structure

```
[PROJECT_CODE]/
├── docs/
│   ├── product/          # PRD files, user stories, feature specs
│   ├── api/              # openapi.yaml, Postman collections
│   ├── architecture/     # system-design.md, ERD, ADRs
│   ├── meetings/         # YYYY-MM-DD meeting notes
│   └── onboarding/       # Team onboarding docs (copied from this repo)
├── designs/
│   ├── wireframes/       # Low-fi exports from Figma
│   ├── mockups/          # Hi-fi screen exports (PNG 2×)
│   ├── assets/           # Icons (SVG), images, logos
│   └── design-system/    # Color tokens, typography documentation
├── by-team/
│   ├── product/          # Working notes, competitive analysis, personas
│   ├── uix/              # figma-links.md, design decisions, handoff notes
│   ├── pm/               # roadmap.md, risk-register.md, sprint plans
│   ├── frontend/         # Component specs, style guide, API integration notes
│   ├── backend/          # DB schema drafts, business logic notes
│   ├── vision/           # Dataset cards, model cards, experiment notes
│   ├── qa/               # Test plans, bug log, UAT checklist
│   ├── devops/           # Infra notes, env variable list (no values)
│   └── mobile/           # api-contracts.md, ux-notes.md
├── reports/
│   ├── qa/               # sprint-[n]-qa-report.md
│   └── sprint/           # sprint-[n]-retro.md, velocity
├── src-link/
│   └── GITHUB.md         # Pointer to repo, branch strategy, clone instructions
└── [PROJECT_CODE]-project-config.md
```

Create a brief `README.md` inside each subfolder to explain its purpose. This helps new team members navigate the structure immediately on Day 1.

---

## 7. Step 5 — Set Up the Design Workspace (Figma)

The Figma workspace is the single source of truth for all visual design decisions. UIX Designers work here; all other team members reference it as viewers.

1. Create a new **Project** in the QTrust Figma organisation named `[PROJECT_NAME]` (e.g., "QTrust Portal")
2. Grant all UIX Designers an **Editor** seat on the project
3. Grant all other team members **Viewer** access — they can inspect designs and copy values but cannot edit
4. UIX Designer creates the 4 standard Figma files:
   - `[PROJECT_CODE] — Design System` — colour tokens, typography scale, component library
   - `[PROJECT_CODE] — Wireframes` — low-fidelity user flows and screen layouts
   - `[PROJECT_CODE] — Hi-Fi Mockups` — production-ready screen designs
   - `[PROJECT_CODE] — Mobile` (if a mobile app is in scope) — separate file for iOS/Android layouts
5. UIX Designer documents all file links in `by-team/uix/figma-links.md` in the Google Drive project folder
6. Verify the Figma MCP in the Cowork project can read and write to the project — test with: `"List my Figma files"` and confirm the new project appears

> **💡 Figma MCP:** With the Figma MCP active in the Cowork project, UIX Designers can prompt Claude to generate wireframes, components, and layouts directly into Figma without switching applications. This is especially powerful in the early design phase when many screens need to be sketched quickly.

---

## 8. Step 6 — Provision Infrastructure

Infrastructure setup depends on the cloud provider recorded in the Project Configuration Sheet. This step is owned and executed by the DevOps Engineer. The IT Head approves the architecture before provisioning begins.

QTrust uses a 3-tier model — **Local → Staging → Production** — defined in full in [`environments-and-promotion.md`](environments-and-promotion.md). Local development runs on each engineer's machine; the first **shared cloud environment** is **staging**, to which `develop` auto-deploys. The goal of this step is to have a working **staging environment** live before the team's first development sprint. **Production** is provisioned later, closer to launch — but its gated deployment pipeline is configured early (see Step 6 detail and `devops.md`).

### Cloud Services Reference

| Resource | GCP | AWS | Azure |
|---|---|---|---|
| Compute | Cloud Run | ECS Fargate | App Service |
| Database | Cloud SQL | RDS | Azure Database |
| Cache / Queue | Memorystore | ElastiCache | Azure Cache for Redis |
| Container Registry | Artifact Registry | ECR | ACR |
| Secrets | Secret Manager | Secrets Manager | Key Vault |
| File Storage | Cloud Storage | S3 | Blob Storage |
| ML Serving | Vertex AI / Cloud Run | SageMaker | Azure Machine Learning |
| CI/CD | GitHub Actions | GitHub Actions | GitHub Actions |

> **⚠️ Infrastructure as Code:** All cloud resources must be provisioned via Terraform (or the IaC tool specified in the Project Config Sheet). Never create resources manually in the cloud console. All IaC files are stored in the `infrastructure/` directory in the GitHub repository and version-controlled like application code.

**Minimum staging environment for Sprint 1:**

- Container or compute instance running the backend application
- Database with schema migrations applied
- CI/CD pipeline triggered on every merge to `develop`, deploying automatically to the **staging** environment
- The **production** pipeline configured as a gated deploy from `main` (manual approval) — even before production traffic exists, so the first release is routine
- All environment variables stored in the cloud secrets manager — no `.env` files committed to the repository
- Basic health-check endpoint returning HTTP 200
- Observability provisioned (see below)

### Observability — Sentry & Datadog

DevOps sets up the QTrust observability stack as part of this step:

- **Sentry** — create one Sentry project per deployable service (`[project-code]-backend`, `-frontend`, `-mobile-android`, `-mobile-ios`, `-ai-serving` — only those in scope). Store each DSN in the cloud secrets manager; engineers integrate the SDK in their service. See [`tools/sentry.md`](tools/sentry.md).
- **Datadog** — provision the Datadog agent via Terraform on all services and create the per-project dashboards (Overview, Infrastructure, Database, AI Model). See [`tools/datadog.md`](tools/datadog.md).
- **Alerts** — route both Sentry and Datadog alerts to `#[project-code]-alerts` in Slack (error spikes, high latency, service down, new fatal errors). The cloud provider's native monitoring (Cloud Monitoring / Cloud Logging) remains the baseline layer beneath them.

---

## 9. Step 7 — Onboard Your Team

QTrust maintains 9 standard onboarding guides — one per team role — in this repository under `teams/`. These are project-agnostic templates that contain placeholders. Before distributing each guide, the IT Head or PM fills in the placeholders using values from the Project Configuration Sheet.

> The Config Sheet is the single source of truth. All placeholder values come from there.

### Onboarding Distribution Table

| Team | Guide | Distribute When | Additional Action Required |
|---|---|---|---|
| Product Development | `teams/product-development.md` | Day 1 | Share Config Sheet + any existing research, competitive analysis, or stakeholder briefs |
| UIX Designer | `teams/uix-designer.md` | Day 1 | Grant Figma Editor seat. Verify Figma MCP is active in Cowork. |
| Project Manager | `teams/project-manager.md` | Day 1 | Grant GitHub Maintainer role + Linear Member. Set up labels, milestones, Kanban board, and Linear cycles. Create the shared Google Calendar; enable Fireflies auto-join. |
| Frontend Engineer | `teams/frontend-engineer.md` | After repo is ready | Confirm dev environment runs locally. Share API base URL. |
| Backend Engineer | `teams/backend-engineer.md` | After repo is ready | Confirm DB migrations run cleanly. Share DB credentials via secrets manager access. |
| Image Vision Engineer | `teams/image-vision-engineer.md` | After repo is ready | Confirm ML environment is set up. Confirm dataset access is granted. |
| QA / QC | `teams/qa-qc.md` | Before Sprint 1 | Confirm test suite runs. Postman collection imported. Staging environment available. |
| DevOps | `teams/devops.md` | Before infrastructure provisioning | Confirm cloud project access + CI/CD secrets configured. Grant Sentry + Datadog admin; create Sentry projects and Datadog dashboards. |
| Mobile Apps Engineer | `teams/mobile-engineer.md` | After repo is ready | Confirm chosen platform (React Native / Flutter / Native). App builds on emulator. |

### Access Checklist Before Distributing Guides

- [ ] QTrust premium Claude account provisioned for this team member
- [ ] Google Drive project folder — Editor access granted
- [ ] `qtrust-id` GitHub organisation — appropriate role assigned (Write for engineers, Triage for QA, Maintainer for PM)
- [ ] Slack — added to project channels (including `#[project-code]-alerts` for engineers/DevOps)
- [ ] Claude Cowork project — invitation sent and accepted
- [ ] Figma access granted (if UIX Designer or team member who needs to inspect designs)
- [ ] ML platform access granted (if Image Vision Engineer)
- [ ] Cloud console access granted to DevOps with least-privilege IAM roles
- [ ] Linear — Member for PM/leads, read for QA and engineers (per the matrix in `tools/README.md`)
- [ ] Google Calendar + Fireflies — required for PM and CTO; recommended for everyone attending meetings
- [ ] Sentry — access to the relevant service projects (Backend, Frontend, Mobile, Image Vision, QA; admin for DevOps)
- [ ] Datadog — access for Backend and DevOps (admin); recommended for Image Vision and QA

> Use the per-role tool matrix and checklists in [`tools/README.md`](tools/README.md) as the authoritative reference for exactly which tools each role needs and at what access level.

---

## 10. Step 8 — Run the Project Kickoff

The kickoff meeting is the moment the entire team assembles for the first time. Its purpose is alignment: everyone leaves with the same understanding of the product, the technical approach, the working agreements, and what needs to happen in Sprint 0.

The PM schedules the kickoff only after Steps 1–7 are complete. Do not run a kickoff before the repository, Cowork project, and Google Drive structure exist — the tools walkthrough portion of the meeting requires them to be live.

### Kickoff Meeting Agenda (90 minutes)

| Duration | Agenda Item | Owner | Goal |
|---|---|---|---|
| 10 min | Welcome & introductions | IT Head / Product Head | Who is in the room and what each person owns on this project |
| 15 min | Product vision & goals | Product Development | Walk through the product vision, target users, and success metrics |
| 15 min | Technical architecture overview | IT Head / Lead Engineer | Explain stack choices — walk through the Project Config Sheet |
| 15 min | Sprint 0 scope & deliverables | Project Manager | Define what must be completed before Sprint 1 can start |
| 10 min | Tools walkthrough | IT Head | Live demo: Cowork project, Google Drive structure, GitHub repo, Figma workspace |
| 10 min | Working agreements | PM + Team | Agree on: standup time, communication channel, PR review SLA, escalation path |
| 10 min | Open questions | All | Surface and log any blockers, unknowns, or risks — add as GitHub Issues immediately |
| 5 min | Next steps | PM | Who does what before the next standup — assign in GitHub Issues before the meeting ends |

Record meeting notes in `docs/meetings/YYYY-MM-DD-kickoff.md` in the Google Drive project folder. The PM is responsible for posting notes within 24 hours.

### Sprint 0 Minimum Deliverables

Sprint 0 is not a development sprint. It is the setup sprint. No product features are built in Sprint 0. The deliverables are infrastructure, tooling, and the first backlog.

- [ ] Project Configuration Sheet finalised and distributed to all team members
- [ ] GitHub repository live with correct branches, labels, and milestones
- [ ] Google Drive folder structure created with README files in each subfolder
- [ ] Figma design system file started — at minimum colour tokens and typography scale defined
- [ ] Local development environment running for all engineers who need it
- [ ] CI/CD pipeline deploying automatically to the **staging** environment on merge to `develop`
- [ ] First PRD written for the highest-priority module
- [ ] Sprint 1 backlog created: GitHub Issues exist, are estimated, labelled, and assigned

---

## 11. Complete Team Workflow

### How All 9 Teams Interact Across a Typical Project Lifecycle

| Phase | Teams Active | Key Handoffs |
|---|---|---|
| Phase 1: Discovery | Product Development, Project Manager | Approved PRD → UIX Designer + PM |
| Phase 2: Design | UIX Designer (primary), Product Dev (review) | Approved mockups → Frontend + Mobile |
| Phase 3: Sprint Planning | Project Manager, Product Development | Sprint backlog → all engineering teams |
| Phase 4: AI Model Development | Image Vision Engineer, Backend | Model `/predict` API endpoint → Backend integration |
| Phase 5: Backend Development | Backend Engineer, Image Vision | REST API + model endpoints → Frontend + Mobile |
| Phase 6: Frontend Development | Frontend Engineer, UIX Designer | Web UI complete → QA |
| Phase 7: Mobile Development | Mobile Apps Engineer, UIX Designer | Mobile app complete → QA |
| Phase 8: Quality Assurance | QA / QC (all teams support) | QA sign-off → DevOps |
| Phase 9: Deployment | DevOps (all teams support) | Production live → all teams notified |

The phases above are not strictly sequential. In practice, Phase 4 (AI model development) and Phase 5 (backend development) often run in parallel. Phase 6 and Phase 7 can also run in parallel once the backend API contract is agreed upon. The Project Manager is responsible for managing these parallel tracks.

### Team Interaction Matrix

This matrix shows who hands off to whom. A ✓ indicates a direct dependency exists: the **From** team produces something the **To** team depends on to proceed.

| From ↓ / To → | Product Dev | UIX | PM | Image Vision | Backend | Frontend | Mobile | QA | DevOps |
|---|---|---|---|---|---|---|---|---|---|
| **Product Development** | — | ✓ | ✓ | | ✓ | | | | |
| **UIX Designer** | | — | | | | ✓ | ✓ | ✓ | |
| **Project Manager** | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Image Vision Engineer** | | | | — | ✓ | | | ✓ | ✓ |
| **Backend Engineer** | | | | | — | ✓ | ✓ | ✓ | |
| **Frontend Engineer** | | | | | | — | | ✓ | |
| **Mobile Apps Engineer** | | | | | | | — | ✓ | |
| **QA / QC** | | | | | | | | — | ✓ |
| **DevOps** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — |

*DevOps → All teams: DevOps provides environments, credentials, and deployment pipelines that every team depends on throughout the project. DevOps must be set up early.*

---

## 12. Team Composition Matrix

Use this matrix when completing Section B of the Project Configuration Sheet to decide which teams to include.

| Team | Include by Default | Include When… | Can Skip When… |
|---|---|---|---|
| Product Development | Always | Every project | Never |
| UIX Designer | Usually | Any user-facing interface (web or mobile) | Pure API services or data-pipeline projects with no UI |
| Project Manager | Usually | 2 or more engineers involved, or duration exceeds 4 weeks | Very small, fixed-scope solo or two-person projects |
| Frontend Engineer | Often | Web UI is in scope | API-only projects, or no web interface in current scope |
| Backend Engineer | Yes | Any server-side logic, database, or API is required | Pure static frontend with no backend logic (rare) |
| Image Vision Engineer | Sometimes | AI, machine learning, or computer vision features are required | No AI/ML functionality in scope |
| QA / QC | Yes | Any product shipped to real users | Internal scripts, proof-of-concept builds, or local-only tools |
| DevOps | Usually | Cloud deployment or CI/CD pipeline is required | No-code deployment platforms, or local-only projects with no deployment |
| Mobile Apps Engineer | Sometimes | An Android or iOS application is required | Web-only projects |

---

## 13. Standard Folder Structure (Reference)

This is the canonical Google Drive folder structure for every QTrust project. Create this structure during Step 4 before distributing team onboarding guides.

```
[PROJECT_CODE]/
├── docs/
│   ├── product/          # PRD files, user stories, feature specs
│   ├── api/              # openapi.yaml, Postman collections
│   ├── architecture/     # system-design.md, ERD, ADRs
│   ├── meetings/         # YYYY-MM-DD meeting notes
│   └── onboarding/       # Team onboarding docs (copied from this repo)
├── designs/
│   ├── wireframes/       # Low-fi exports from Figma
│   ├── mockups/          # Hi-fi screen exports (PNG 2×)
│   ├── assets/           # Icons (SVG), images, logos
│   └── design-system/    # Color tokens, typography documentation
├── by-team/
│   ├── product/          # Working notes, competitive analysis, personas
│   ├── uix/              # figma-links.md, design decisions, handoff notes
│   ├── pm/               # roadmap.md, risk-register.md, sprint plans
│   ├── frontend/         # Component specs, style guide, API integration notes
│   ├── backend/          # DB schema drafts, business logic notes
│   ├── vision/           # Dataset cards, model cards, experiment notes
│   ├── qa/               # Test plans, bug log, UAT checklist
│   ├── devops/           # Infra notes, env variable list (no values)
│   └── mobile/           # api-contracts.md, ux-notes.md
├── reports/
│   ├── qa/               # sprint-[n]-qa-report.md
│   └── sprint/           # sprint-[n]-retro.md, velocity
├── src-link/
│   └── GITHUB.md         # Pointer to repo, branch strategy, clone instructions
└── [PROJECT_CODE]-project-config.md
```

---

## 14. Cowork Project Instructions Template

Paste this into Claude Desktop → Project Settings → Instructions. Replace every `[PLACEHOLDER]` with the actual value from the Project Configuration Sheet. Do not leave any placeholder text in the final instructions.

```
You are assisting the engineering team at QTrust (PT Langgeng Sukses Abadi Tekhnologi).

PROJECT: [PROJECT_NAME] ([PROJECT_CODE])
DESCRIPTION: [One-paragraph description of the product and its users]

TECH STACK:
- Backend:     [FRAMEWORK] [VERSION]
- Frontend:    [FRAMEWORK] + [CSS_FRAMEWORK]
- Database:    [DATABASE]
- Cache/Queue: [CACHE_SYSTEM or None]
- Mobile:      [APPROACH or None]
- AI/Vision:   [FRAMEWORK or None]
- Cloud:       [PROVIDER] / [DEPLOYMENT_TARGET]
- Repository:  [REPO_URL]

DOCUMENTATION:
- Language: English (all documents, code comments, commit messages)
- Format: Markdown (.md) files
- Repo: https://github.com/qtrust-id/ONBOARDING

PROJECT CONFIG SHEET:
[GOOGLE_DRIVE_PATH]/[PROJECT_CODE]-project-config.md

YOUR ROLE:
Act as a co-pilot for every team member on this project.
- Draft PRDs, user stories, API specs, and architecture documents
- Generate, review, and explain code in the project's tech stack
- Write and interpret tests
- Assist with CI/CD configuration and infrastructure
- Generate designs in Figma when the UIX Designer requests it (Figma MCP is active)
- Always produce documentation as Markdown (.md) files in clear, professional English

Always read the Project Configuration Sheet when a team member asks about
project-specific details (stack, URLs, team contacts, timeline).
```

---

## 15. Governance & Standards

These standards apply to every QTrust project. They are not optional. If a project-specific decision conflicts with these standards, escalate to the IT Head before proceeding.

### Documentation Standards

- All project documentation must be written in **clear, professional English**
- All documents must be in **Markdown (`.md`) format**, stored in the Google Drive project folder
- Document every significant architectural decision as an **Architecture Decision Record (ADR)** in `docs/architecture/adr/` — use the format: `adr-[NNN]-[title].md`
- Use Claude Desktop to generate initial drafts; the responsible team member must always review and approve before a document is finalised or distributed

### Code Standards

- Use **Conventional Commits** for all commit messages: `feat`, `fix`, `docs`, `test`, `refactor`, `chore` — followed by a scope and a short description in English
- **No direct pushes** to `main` or `develop` — all changes via Pull Request with a minimum of 1 approved reviewer
- **CI must pass** (all tests green + build succeeds) before any PR can merge
- **No secrets in code** — use the cloud provider's secrets manager; never commit `.env` files with real values
- All code comments in **English** only
- Each PR should reference the GitHub Issue it addresses (e.g., `Closes #42`)

### Security Standards

- Never store API keys, passwords, or credentials in the repository — not even in the commit history
- Never share credentials via chat, email, Google Drive, or Slack
- Apply least-privilege IAM roles for all cloud services — each service account has only the permissions it needs
- All production traffic must be served over HTTPS only
- Rotate secrets annually, or immediately when a team member with credential access leaves the project or the company
- A security review (as described in the ONBOARDING repository) is required before any deployment to staging or production when the project handles PII or Confidential data

### AI-Assisted Workflow

> **⚠️ Claude assists, humans decide.** Every AI-generated output — document, code, or design — must be reviewed and approved by the responsible team member before it is used, committed, or distributed. "Claude generated it" is not an acceptable explanation for a bug, an incorrect requirement, or a broken design. The team member who commits or publishes AI-generated output takes full ownership of it.

---

## 16. Master Setup Checklist

Use this checklist to track progress through the full project setup process. Every item must be checked before you declare setup complete and Sprint 1 begins.

### Project Configuration

- [ ] Config Sheet completed and approved by both IT Head and Product Head
- [ ] Project code, name, and description finalised
- [ ] Team composition confirmed — all members identified and available
- [ ] Technology stack decisions documented and approved
- [ ] Timeline and milestones agreed with relevant stakeholders

### Claude Cowork

- [ ] Cowork project created with the correct name (`[COMPANY_CODE]-[PROJECT_CODE]`)
- [ ] Project instructions filled in from the Section 14 template — no placeholders remaining
- [ ] Workspace folder connected to the local Google Drive sync path
- [ ] Figma MCP verified and working (if UIX Designer is on the team)
- [ ] GitHub MCP verified and working
- [ ] Project-relevant MCPs connected and verified per the matrix in `tools/README.md` (Linear, Google Calendar, Fireflies, and — once provisioned — Sentry and Datadog)
- [ ] All team members added to the Cowork project and have accepted the invitation

### GitHub

- [ ] Repository created under `qtrust-id` organisation (Private)
- [ ] `develop` branch created from `main`
- [ ] Branch protection rules applied to `main` and `develop`
- [ ] All labels created: module, type, priority, team, status
- [ ] All milestones created with target dates
- [ ] Kanban board created with standard columns
- [ ] All team members added with correct roles
- [ ] README updated with project description and Config Sheet link

### Google Drive

- [ ] Project folder created in QTrust Shared Drive
- [ ] Folder shared with all team members (Editor access)
- [ ] Standard folder structure created with README files in each subfolder
- [ ] Config Sheet saved in the folder root as `[PROJECT_CODE]-project-config.md`
- [ ] Team onboarding guides linked from `docs/onboarding/`
- [ ] `src-link/GITHUB.md` created pointing to the repository

### Figma

- [ ] Figma project created in the QTrust Figma organisation
- [ ] UIX Designers granted Editor seat
- [ ] All other team members granted Viewer access
- [ ] Four initial design files created (Design System, Wireframes, Hi-Fi Mockups, Mobile if applicable)
- [ ] All file links documented in `by-team/uix/figma-links.md`
- [ ] Figma MCP confirmed working from the Cowork project

### Infrastructure

- [ ] Cloud project / account created and billing configured
- [ ] Staging environment provisioned: compute + database + cache
- [ ] CI/CD pipeline deploying automatically to **staging** on merge to `develop`
- [ ] Production pipeline configured as a gated deploy from `main`
- [ ] GitHub Environments created: `staging` (no reviewers) and `production` with a **Required reviewers** protection rule (IT Head / Tech Lead) — this enforces the manual approval gate on production deploys (Settings → Environments → `production` → Required reviewers)
- [ ] All environment variable names listed in `by-team/devops/` (values in secrets manager only)
- [ ] All secrets stored in the cloud provider's secrets manager
- [ ] Basic health-check endpoint returning HTTP 200
- [ ] Sentry projects created per service, DSNs stored in the secrets manager
- [ ] Datadog agent provisioned and per-project dashboards created
- [ ] Monitoring and alerting configured for the staging environment, with Sentry and Datadog alerts routed to `#[project-code]-alerts` in Slack

### Project Management & Meetings

- [ ] Linear team created with cycles mapped to the sprint milestones
- [ ] Shared `[PROJECT_CODE] — Team` Google Calendar created and shared with the team
- [ ] Sprint 1 ceremonies scheduled; `fred@fireflies.ai` invited to all recurring ceremonies

### Team Onboarding

- [ ] All team members have QTrust premium Claude accounts provisioned
- [ ] All team members received the correct onboarding guide for their role
- [ ] All team members granted their role's tool access per the matrix in `tools/README.md`
- [ ] All team members have confirmed they have read the Project Configuration Sheet
- [ ] All team members confirmed their local development environment is working

### Kickoff

- [ ] Kickoff meeting held — notes recorded in `docs/meetings/YYYY-MM-DD-kickoff.md`
- [ ] Sprint 0 started — all Sprint 0 issues created, labelled, and assigned in GitHub
- [ ] Sprint 1 backlog defined — issues estimated and ready for Sprint 1 planning

> **✅ Setup is complete** when every box above is checked, every team member's environment is confirmed working, and Sprint 0 has started.

---

*Maintained by QTrust Engineering · https://github.com/qtrust-id/ONBOARDING*
