# Environments & Promotion Workflow

> **Audience:** All teams
> **Purpose:** The single source of truth for how code and changes move from a local machine to production across every QTrust project.
> **Version:** 1.0 — June 2026

---

## 1. Philosophy

Every QTrust project uses the same three environments, in the same order. Code is written and tested **locally**, integrated and validated on **staging**, and only then released to **production**. Nothing reaches production without passing through staging first.

> **Core rule:** Local → Staging → Production. Never skip a tier. A change is only as trustworthy as the last environment it survived.

This applies to every team — Backend, Frontend, Mobile, Image Vision, QA, and DevOps all operate against the same environment definitions and the same promotion gates.

---

## 2. The Three Environments

| | **Local** | **Staging** | **Production** |
|---|---|---|---|
| **Runs on** | Each engineer's machine (e.g., MacBook) via Docker | Shared cloud (e.g., GCP Cloud Run) | Shared cloud (e.g., GCP Cloud Run) |
| **Purpose** | Build and unit-test features in isolation | Integration, QA, and UAT against real cloud services | Live system used by real users |
| **Branch** | `feature/*`, `fix/*` | `develop` | `main` |
| **How it deploys** | Manual (`docker compose up`, run the dev server) | **Automatic** on merge to `develop` | **Gated** — manual approval on merge to `main` |
| **URL pattern** | `http://localhost:[port]` | `https://[project-code]-staging.qtrust.id` | `https://[project-code].qtrust.id` |
| **`APP_ENV`** | `local` | `staging` | `production` |
| **Data** | Disposable seed data | Realistic but non-real test data | Real user data — handle with care |
| **Debug tools** | On (Telescope, debug toolbars, verbose logs) | Off (or restricted) | Off — `APP_DEBUG=false` |
| **Min instances** | n/a | 0 (scales to zero) | ≥ 1 (always warm) |

> A project may add a separate shared integration/dev environment later if scale demands it, but the default and the standard every project starts with is this 3-tier model.

---

## 3. Branch ↔ Environment Mapping

```
feature/* , fix/*   ──►  Local        (manual; the engineer's machine)
        │  PR + CI
        ▼
develop             ──►  Staging       (auto-deploy on merge)
        │  PR + approval
        ▼
main                ──►  Production    (gated deploy: manual approval)
```

| Branch | Deploys to | Trigger | Who merges |
|---|---|---|---|
| `feature/*`, `fix/*` | Local only | Manual, on the engineer's machine | Engineer |
| `develop` | **Staging** | Automatic on every merge | Any engineer, via reviewed PR |
| `release/*` *(optional)* | — | Stabilisation branch cut from `develop` for a release candidate | PM / Tech Lead |
| `main` | **Production** | **Manual approval** after PR from `develop` (or `release/*`) | IT Head / Tech Lead approves |

Branch protection: no direct pushes to `develop` or `main`; both require a passing CI run and at least one approving review.

---

## 4. The Promotion Flow

```
1. LOCAL (develop on your MacBook)
   ├─ Branch from develop: feature/[module]-[description]
   ├─ Build the feature; run unit + feature tests locally (docker compose up)
   └─ Open a PR targeting develop
        │  CI runs: lint + tests + build. Must be green. 1+ approval.
        ▼
2. STAGING (auto-deployed from develop)
   ├─ Merge to develop → CI/CD auto-deploys to staging
   ├─ Engineer verifies the feature works on staging
   ├─ QA runs exploratory + regression testing against staging
   ├─ UIX confirms the build matches Figma; Product runs UAT
   └─ Sign-off: QA approves, no open critical/high bugs
        │  Open a PR develop → main (optionally via release/*)
        ▼
3. PRODUCTION (gated deploy from main)
   ├─ Pre-production checklist complete (see Section 6)
   ├─ IT Head / Tech Lead approves the deployment in GitHub
   ├─ CI/CD deploys to production
   └─ DevOps monitors Sentry + Datadog for the first 48 hours
```

Each arrow is a **gate** — the change does not advance until the gate's condition is met.

---

## 5. Per-Environment Configuration & Secrets

The same code runs in all three environments. Only configuration changes.

| | Local | Staging | Production |
|---|---|---|---|
| **Config source** | `.env` (copied from `.env.example`) | Secrets manager (e.g., GCP Secret Manager) | Secrets manager |
| **Committed to Git?** | Only `.env.example` (no real values) | Never | Never |
| **Set by** | Each engineer | DevOps (via IaC + secrets manager) | DevOps (via IaC + secrets manager) |

Rules that apply everywhere:

- **No secrets in code or Git** — not in `.env` files with real values, not in the image, not in commit history. Staging and production secrets live only in the cloud secrets manager and are injected at runtime.
- **Non-sensitive config** (e.g., `APP_ENV`, `APP_URL`, log level) is set as plain environment variables on the service; secrets (DB password, API keys, DSNs) come from the secrets manager.
- Each engineer keeps separate local config for their feature work; staging and production config is owned by DevOps and identical in shape, differing only in values.

---

## 6. Gates & Sign-offs

**Gate 1 — Local → Staging (merge to `develop`)**

- [ ] CI is green: lint passes, all tests pass, build succeeds
- [ ] At least one approving code review on the PR
- [ ] PR references its GitHub Issue

**Gate 2 — Staging → Production (merge to `main` + deploy approval)**

- [ ] Feature verified working on staging by the implementing engineer
- [ ] QA sign-off — no open `priority:critical` or `priority:high` bugs
- [ ] UIX sign-off (matches Figma) and Product/UAT sign-off where applicable
- [ ] Database migrations reviewed — no destructive change without a rollback plan
- [ ] Recent database backup confirmed (within 24h)
- [ ] Rollback plan documented — previous image tag noted
- [ ] IT Head / Tech Lead approves the production deployment
- [ ] Team notified of the deployment window

---

## 7. Per-Team Responsibilities Across Environments

| Team | Local | Staging | Production |
|---|---|---|---|
| **Backend / Frontend / Image Vision** | Build + unit/feature tests locally; keep `.env.example` current | Verify the feature on staging before marking the issue *Ready for QA* | Triage Sentry/Datadog after release; fix forward via the same flow |
| **Mobile** | Point the app at the **Development**/local API; build on emulator | Point a test build at the **Staging** API; submit to Play Internal / TestFlight | Point release build at the **Production** API |
| **QA / QC** | Review acceptance criteria; help author tests | **Staging is the official test environment** — exploratory, regression, UAT, performance/SLA checks | Smoke-test after release; verify no new Sentry errors |
| **UIX Designer** | — | Design QA against the staging build vs Figma | Confirm visual correctness post-release |
| **Project Manager** | — | Track *Ready for QA* / *In QA* on staging; coordinate UAT | Schedule the release window; confirm sign-offs before approval |
| **DevOps** | Provide `docker compose` + `.env.example` | Own the auto-deploy pipeline to staging | Own the gated production deploy, approval rule, monitoring, and rollback |

---

## 8. Monitoring Per Environment

- **Sentry** — tag every event with its environment (`local` / `staging` / `production`) so errors are filterable per tier. QA uses the `staging` scope to catch regressions before release; DevOps and engineers watch `production` after a deploy. See [`tools/sentry.md`](tools/sentry.md).
- **Datadog** — staging and production are separate scopes; validate performance SLAs on staging during load tests, and watch production latency/error rate live. See [`tools/datadog.md`](tools/datadog.md).
- Alerts from both fire to `#[project-code]-alerts` in Slack.

---

## 9. Rollback

Production issues are resolved by rolling back, not by hot-fixing live:

1. Identify the previous known-good image tag (recorded in the deployment notes from the last release).
2. Re-deploy that tag to production (DevOps runbook in `by-team/devops/infra-notes.md`).
3. Reproduce and fix the issue through the normal Local → Staging → Production flow.

Never patch production directly. Every fix re-enters at the local tier.

---

## 10. Environment Provisioning Order

| Environment | Provisioned | Owner |
|---|---|---|
| **Local** | Day 1 — every engineer sets it up from the repo `README` and `.env.example` | Each engineer |
| **Staging** | Before Sprint 1 — `develop` must auto-deploy somewhere the team can test | DevOps (Step 6 of the New Project Setup Guide) |
| **Production** | Before the launch/deployment phase — with the gated pipeline and approval rule in place | DevOps |

> Staging exists from Sprint 1 so that integration and QA happen continuously, not in a rush at the end. Production is stood up later, closer to launch, but its gated pipeline is configured early so the first production deploy is routine, not novel.

---

*Maintained by QTrust Engineering · https://github.com/qtrust-id/ONBOARDING*
