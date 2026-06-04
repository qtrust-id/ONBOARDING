# Project Configuration Sheet — [PROJECT_NAME]

> **Version:** 1.0  
> **Status:** Draft / Approved  
> **Author:** [Name]  
> **Approved by:** [IT Head Name]  
> **Last Updated:** YYYY-MM-DD

---

## A. Project Identity

| Field | Value |
|---|---|
| **Project Name** | [Full project name] |
| **Project Code** | [Short code, e.g., QT-PORTAL, QT-SCT] |
| **Description** | [One-sentence description of what this project delivers] |
| **Business Owner** | [Name — Department] |
| **IT Lead** | [Name] |
| **Status** | Active / On Hold / Completed |
| **Start Date** | YYYY-MM-DD |
| **Target Launch Date** | YYYY-MM-DD |
| **GitHub Repository** | [https://github.com/qtrust-id/repo-name] |
| **Cowork Project** | [Project name in Claude Cowork] |

---

## B. Team Composition

| Role | Name | Responsibilities |
|---|---|---|
| **IT Head / Tech Lead** | [Name] | Architecture decisions, code review, cross-team coordination |
| **Product Development** | [Name] | PRD authoring, sprint planning, stakeholder communication |
| **Backend Engineer** | [Name] | API, business logic, database, integrations |
| **Frontend Engineer** | [Name] | Web UI (if applicable) |
| **Mobile Engineer (Android)** | [Name] | Android app (if applicable) |
| **Mobile Engineer (iOS)** | [Name] | iOS app (if applicable) |
| **Image / Vision Engineer** | [Name] | AI/ML models and serving API (if applicable) |
| **UIX Designer** | [Name] | UI design in Figma |
| **QA / QC Engineer** | [Name] | Test planning, execution, bug reporting |
| **DevOps Engineer** | [Name] | CI/CD pipelines, cloud infrastructure, deployment |
| **Project Manager** | [Name] | Timeline tracking, risk management, meeting facilitation |

---

## C. Technology Stack

### C1. Backend

| Field | Value |
|---|---|
| **Language** | [e.g., PHP 8.3] |
| **Framework** | [e.g., Laravel 13.x] |
| **Database** | [e.g., PostgreSQL 16] |
| **Cache / Queue** | [e.g., Redis 7] |
| **Authentication** | [e.g., Laravel Sanctum] |
| **Background Jobs** | [e.g., Laravel Horizon + Redis queues] |

### C2. Frontend

| Field | Value |
|---|---|
| **Language** | [e.g., TypeScript] |
| **Framework** | [e.g., Next.js 15 / React 19] |
| **UI Library** | [e.g., Tailwind CSS + shadcn/ui] |
| **State Management** | [e.g., Zustand / Redux Toolkit] |
| **API Client** | [e.g., Axios / TanStack Query] |

### C3. Mobile

| Field | Value |
|---|---|
| **Platform** | [Android / iOS / Both / N/A] |
| **Language** | [e.g., Kotlin (Android), Swift (iOS)] |
| **Architecture Pattern** | [e.g., MVVM] |
| **Networking** | [e.g., Retrofit (Android), URLSession (iOS)] |
| **Minimum OS Version** | [e.g., Android 8.0 (API 26) / iOS 16] |

### C4. AI / Vision (complete if applicable)

| Field | Value |
|---|---|
| **AI Task Type** | [Classification / Object Detection / OCR / Anomaly Detection / N/A] |
| **Performance Target — Accuracy** | [e.g., mAP ≥ 0.85 / F1 ≥ 0.90 / N/A] |
| **Performance Target — Latency** | [e.g., p95 ≤ 150ms / N/A] |
| **ML Framework** | [PyTorch / TensorFlow / N/A] |
| **Export Format** | [ONNX / TorchScript / TFLite / N/A] |
| **Dataset Source** | [Internal / Roboflow / Partner / Synthetic / N/A] |
| **Training Infrastructure** | [GCP Vertex AI / AWS SageMaker / Local GPU / N/A] |
| **Model Serving** | [GCP Cloud Run / Vertex AI Prediction / N/A] |
| **Integration Method** | [REST API / gRPC / On-device SDK / N/A] |

### C5. Cloud & Infrastructure

| Field | Value |
|---|---|
| **Cloud Provider** | [GCP / AWS / Azure / Multi-cloud] |
| **Compute** | [e.g., GCP Cloud Run (backend), Cloud Run (AI serving)] |
| **Database Hosting** | [e.g., GCP Cloud SQL — PostgreSQL] |
| **Object Storage** | [e.g., GCP Cloud Storage] |
| **CDN** | [e.g., Cloudflare / GCP Cloud CDN / N/A] |
| **CI/CD** | [e.g., GitHub Actions] |
| **Container Registry** | [e.g., GCP Artifact Registry] |
| **Secrets Management** | [e.g., GCP Secret Manager / HashiCorp Vault] |
| **Monitoring** | [e.g., GCP Cloud Monitoring + Sentry] |

---

## D. Repository & Infrastructure

| Field | Value |
|---|---|
| **GitHub Organization** | [https://github.com/qtrust-id] |
| **Backend Repo** | [repo-name] |
| **Frontend Repo** | [repo-name or N/A] |
| **Mobile Repo** | [repo-name or N/A] |
| **AI / Vision Repo** | [repo-name or N/A] |
| **Default Branch** | `main` |
| **Branch Protection** | Require PR + 1 approval before merge to `main` |
| **CI on PR** | Lint, tests, build |
| **Deployment: Staging** | Auto-deploy on merge to `develop` |
| **Deployment: Production** | Manual promote via GitHub Actions workflow |

### Environments

| Environment | URL | Branch |
|---|---|---|
| **Local** | http://localhost:[port] | Any |
| **Staging** | https://[project]-staging.qtrust.id | `develop` |
| **Production** | https://[project].qtrust.id | `main` |

---

## E. Design & Documentation

| Field | Value |
|---|---|
| **Figma File** | [https://figma.com/design/...] |
| **Figma Project** | [Project name in Figma] |
| **Design System** | [QTrust Design System / Custom / TBD] |
| **API Documentation** | [URL or path, e.g., docs/api/openapi.yaml] |
| **PRD Location** | [Google Drive folder URL] |
| **Meeting Notes** | [Google Drive folder URL] |
| **By-Team Folder** | [Google Drive folder URL — organized by team role] |
| **Document Language** | English |
| **Document Format** | PDF (final deliverables) / Markdown (working documents) |

---

## F. Timeline & Milestones

| Milestone | Target Date | Owner | Status |
|---|---|---|---|
| Project Config Sheet approved | YYYY-MM-DD | [IT Head] | ☐ |
| Design (Figma) — first draft complete | YYYY-MM-DD | [UIX Designer] | ☐ |
| Backend — core API endpoints complete | YYYY-MM-DD | [Backend Engineer] | ☐ |
| Frontend / Mobile — MVP screen complete | YYYY-MM-DD | [FE / Mobile Engineer] | ☐ |
| AI model — baseline model trained | YYYY-MM-DD | [Vision Engineer] | ☐ |
| Internal QA complete | YYYY-MM-DD | [QA Engineer] | ☐ |
| Staging release | YYYY-MM-DD | [DevOps] | ☐ |
| Production launch | YYYY-MM-DD | [IT Head / PM] | ☐ |

---

## G. Communication

| Channel | Purpose | Members |
|---|---|---|
| **[Team chat channel name]** | Daily team communication | All team members |
| **[Standup meeting]** | Daily standup — 15 minutes | All team members |
| **[Sprint planning meeting]** | Sprint kickoff and planning | All team members |
| **[Sprint review meeting]** | Demo and retrospective | All team members + stakeholders |
| **[Escalation path]** | Blockers and critical issues | [IT Head] → [Business Owner] |

| Meeting | Cadence | Duration | Platform |
|---|---|---|---|
| Daily Standup | Every weekday | 15 min | [e.g., Google Meet] |
| Sprint Planning | Every 2 weeks | 1 hour | [e.g., Google Meet] |
| Sprint Review | Every 2 weeks | 1 hour | [e.g., Google Meet] |
| Ad-hoc Technical | As needed | 30–60 min | [e.g., Google Meet] |

---

## H. Compliance & Security

| Requirement | Detail | Owner |
|---|---|---|
| **Data Classification** | [Public / Internal / Confidential / Restricted] | [IT Head] |
| **PII Handling** | [Does this project process personal data? Yes / No] | [IT Head / Legal] |
| **Authentication Standard** | [e.g., JWT via Laravel Sanctum, min token TTL 15min] | [Backend Engineer] |
| **API Security** | [e.g., All endpoints require Bearer token except /login and /health] | [Backend Engineer] |
| **Secrets Storage** | [No secrets in Git; use GCP Secret Manager / .env not committed] | [All engineers] |
| **Data Residency** | [e.g., All data stored in GCP asia-southeast2 (Jakarta)] | [DevOps] |
| **Vulnerability Scanning** | [e.g., GitHub Dependabot + Snyk on CI] | [DevOps] |
| **Penetration Testing** | [Scheduled? Date: YYYY-MM-DD / N/A] | [IT Head] |
| **Regulatory Requirements** | [e.g., OJK, UU PDP, Barantin traceability requirements / N/A] | [Business Owner] |

---

## Distribution

| Recipient | Date Shared | Acknowledged |
|---|---|---|
| [Name — Team] | YYYY-MM-DD | ☐ |
| [Name — Team] | YYYY-MM-DD | ☐ |
| [Name — Team] | YYYY-MM-DD | ☐ |
| [Name — Team] | YYYY-MM-DD | ☐ |
| [Name — Team] | YYYY-MM-DD | ☐ |

*All team members must acknowledge they have read this document before starting work.*
