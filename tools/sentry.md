# Tool Setup Guide — Sentry MCP

> **Required for:** Backend Engineer, Frontend Engineer, Mobile Apps Engineer, Image Vision Engineer, QA / QC, DevOps, CTO
> **Role:** Production error and performance monitoring. MCP integration lets Claude query errors, analyse stack traces, and track release health directly from conversations.

---

## 1. What It Is

Sentry captures, groups, and prioritises every error and exception that occurs in your application — in production, staging, and development environments. When something breaks in production, Sentry tells you exactly what failed, where in the code it happened, what the user was doing, and how many users were affected.

The Sentry MCP integration allows Claude to query current errors and help with debugging: *"What are the top 5 unresolved errors this week?"* or *"Did the last deployment introduce any new errors?"*

---

## 2. Who Needs This

| Role | Why They Need Sentry |
|---|---|
| CTO / IT Head | Production health overview across all projects |
| Backend Engineer | Server-side error tracking, API failures, database exceptions |
| Frontend Engineer | JavaScript errors, network failures, React component crashes |
| Mobile Apps Engineer | Mobile crash reporting (Android ANR, iOS crashes, Flutter errors) |
| Image Vision Engineer | Model serving errors, inference failures |
| QA / QC | Identify bugs introduced by new releases; verify fixes |
| DevOps | Correlate deployment events with error spikes |

---

## 3. Account & Project Setup

1. Go to [sentry.io](https://sentry.io) and sign up with your `@qtrust.id` Google account
2. Ask the IT Head to add you to the QTrust Sentry organisation
3. Accept the invitation email
4. You will see the QTrust Sentry organisation with projects listed per service

---

## 4. Sentry Project Structure

Each deployable service gets its own Sentry project:

| Sentry Project | Monitors |
|---|---|
| `[project-code]-backend` | Laravel / Django / Node server errors |
| `[project-code]-frontend` | JavaScript / React / Next.js browser errors |
| `[project-code]-mobile-android` | Android crashes and ANRs |
| `[project-code]-mobile-ios` | iOS crashes |
| `[project-code]-ai-serving` | Model serving API errors |

The DevOps engineer creates these projects during infrastructure setup (Step 6 of the New Project Setup Guide).

---

## 5. SDK Integration (For Engineers)

Sentry must be integrated into the application code. This is set up by the Backend / Frontend / Mobile engineer for their respective services.

**Backend (Laravel example):**
```bash
composer require sentry/sentry-laravel
```
```env
SENTRY_LARAVEL_DSN=https://[key]@[org].ingest.sentry.io/[project-id]
```

**Frontend (Next.js / React example):**
```bash
npx @sentry/wizard@latest -i nextjs
```

**Flutter:**
```yaml
# pubspec.yaml
dependencies:
  sentry_flutter: ^8.0.0
```

> The DSN (connection string) for each project is stored in GCP Secret Manager. Ask the DevOps engineer for the DSN — never hardcode it in source files.

---

## 6. Connecting Sentry MCP in Claude Desktop

1. Open Claude Desktop → **Settings** → **Integrations**
2. Find **Sentry** and click **Connect**
3. Authorise with your `@qtrust.id` account and select the QTrust organisation
4. Return to Claude Desktop — the connection should show **Connected**

---

## 7. Verification Test

Run these prompts in Claude after connecting to confirm the integration is working:

```
"List open issues in the [project-code]-backend Sentry project."
"Show me the top 5 most frequent errors in the last 7 days."
"Are there any new issues introduced after the last deployment?"
```

---

## 8. Usage Examples by Role

**CTO — Daily health check:**
```
"Query Sentry for all projects in the QTrust organisation.
For each project: how many unresolved errors, how many new errors this week,
and what is the crash-free session rate?"
```

**Backend / Frontend Engineer — Issue investigation:**
```
"Get the details of Sentry issue [ID].
Show me the stack trace, affected users, and when it first occurred."
```

**Backend / Frontend Engineer — Deployment verification:**
```
"I just deployed a fix for [issue]. Search Sentry to confirm the error rate
for [error name] has dropped in the last 30 minutes."
```

**QA / QC:**
```
"After my test run, are there any new Sentry errors in the staging environment
for [project-code]-backend that weren't there before?"
```

**DevOps:**
```
"The last deployment was 20 minutes ago. Have any new Sentry issues been created
since then in [project-code]-backend or [project-code]-frontend?"
```

---

## 9. Alerting Setup

Configure Sentry to automatically notify the `#[project-code]-alerts` Slack channel for:

- Any new **Fatal** error
- Any error affecting > 10 users
- Error rate spike > 50% above baseline
- A previously resolved issue re-occurring

DevOps sets this up via Sentry → **Alerts** → **Create Alert Rule** → Slack notification.

---

## 10. First-Day Checklist

- [ ] Sentry account created with `@qtrust.id` Google SSO
- [ ] Added to QTrust Sentry organisation
- [ ] Can see relevant Sentry projects for your service
- [ ] Sentry SDK integrated into your service's codebase (engineers only)
- [ ] DSN stored in GCP Secret Manager, not in code
- [ ] Sentry MCP connected in Claude Desktop (run verification test)
- [ ] Slack alert rules configured for `#[project-code]-alerts` (DevOps sets this)
