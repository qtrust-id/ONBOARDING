# Tool Setup Guide — Slack MCP

> **Required for:** All team members
> **Role:** Team communication hub — daily standups, sprint discussions, incident alerts, and cross-team coordination. MCP integration lets Claude read channel summaries and search discussions directly from conversations.

---

## 1. What It Is

Slack is QTrust's primary team communication platform. The Slack MCP integration allows Claude to search messages, read channel discussions, and post summaries — enabling you to ask Claude questions like "What blockers did the backend team mention in standup this week?" without manually scrolling through channels.

For the CTO specifically, Slack MCP combined with Fireflies MCP provides full team pulse visibility through Claude.

---

## 2. Who Needs This

All team members — Slack is required for daily communication. MCP connection is recommended for everyone; most critical for CTO, PM, and Tech Leads.

| Role | Slack Usage | MCP Priority |
|---|---|---|
| CTO / IT Head | Monitor all channels, receive escalations | Required |
| Project Manager | Run standups, track blockers, coordinate teams | Required |
| All Engineers | Daily standup, PR notifications, deployment alerts | Recommended |
| UIX Designer | Design feedback, review requests | Recommended |

---

## 3. Account & Workspace Setup

1. Ask the IT Head for an invitation to the QTrust Slack workspace (`qtrust.slack.com` or similar)
2. Accept the invitation via email
3. Download Slack Desktop from [slack.com/downloads](https://slack.com/downloads/) for a better experience than the browser version
4. Sign in with your `@qtrust.id` Google account (Slack supports Google SSO)

---

## 4. Recommended Channels to Join

Join these channels when you first access the workspace:

| Channel | Purpose | Who Should Join |
|---|---|---|
| `#general` | Company-wide announcements | Everyone |
| `#engineering` | Cross-team technical discussions | All engineers |
| `#[project-code]-general` | Project-specific team discussion | Project team |
| `#[project-code]-standup` | Daily async standup updates | Project team |
| `#[project-code]-deployments` | CI/CD and deployment notifications | Engineers + PM |
| `#[project-code]-alerts` | Sentry errors and Datadog alerts (automated) | Engineers + PM + CTO |
| `#design-feedback` | UIX design reviews | UIX + Product + Frontend |
| `#incidents` | Production incidents (auto-created by PagerDuty/Datadog) | DevOps + Leads + CTO |

> The IT Head creates project-specific channels when a new project is set up. Ask the PM for the list of channels for your project.

---

## 5. Connecting Slack MCP in Claude Desktop

1. Open Claude Desktop → **Settings** → **Integrations**
2. Find **Slack** and click **Connect**
3. You will be redirected to Slack to authorise Claude's access
4. Select the QTrust workspace from the dropdown
5. Grant the requested permissions (read messages, search, post to channels)
6. Return to Claude Desktop — connection should show **Connected**

---

## 6. Verification Test

Run these prompts in Claude Desktop to confirm the integration is working:

```
"Search Slack for messages about [blocker or topic] in the last 7 days."
"Summarise the last 10 messages in #[project-code]-standup."
"What decisions were made in #engineering this week?"
```

---

## 7. Usage Examples by Role

**CTO — Morning brief:**

```
"Summarise all standup updates posted in #[project-code]-standup since Monday.
Highlight any blockers or escalations."
```

**Project Manager:**

```
"List all messages in #[project-code]-general that mention 'blocked' or 'blocker' this sprint."
"Who hasn't posted their standup update in #[project-code]-standup today?"
```

**Engineer:**

```
"Search #engineering for previous discussions about [technical topic]."
"Has anyone in #[project-code]-general mentioned an issue with the [feature] API?"
```

---

## 8. Standup Format (Async Standup)

Post in `#[project-code]-standup` every morning:

```
*[Your Name] — [Date]*
✅ Done: [what you completed yesterday]
🔨 Doing: [what you're working on today]
🚧 Blocked: [any blockers, or "None"]
```

---

## 9. First-Day Checklist

- [ ] Slack workspace invitation accepted
- [ ] Slack Desktop installed
- [ ] Signed in with `@qtrust.id` Google SSO
- [ ] Joined all relevant channels for your project and role
- [ ] Slack MCP connected in Claude Desktop
- [ ] Can search and read channels via Claude (run verification test)
