# QTrust Tool Stack — Complete Setup Guide

> **Audience:** All QTrust team members  
> **Purpose:** Every tool used across all QTrust projects — who needs what, why, and how to activate it in Claude Desktop.

---

## Why This Matters

Every team member at QTrust works inside **Claude Desktop** as their primary AI co-pilot. Claude's power multiplies when it is connected to your tools via **MCP (Model Context Protocol)** integrations. With the right tools connected, Claude can:

- Pull your sprint issues from Linear and GitHub without you leaving the conversation
- Schedule sprint ceremonies in Google Calendar, which Fireflies auto-joins and transcribes
- Summarise meeting transcripts from Fireflies and surface blockers from Slack
- Query production errors from Sentry and infrastructure metrics from Datadog
- Read Figma designs and generate code that matches them precisely

**MCP connections are per-user.** Each team member must connect their own tools in Claude Desktop. This document tells you exactly which tools to connect and in what order.

---

## Tool Overview

| Tool | Purpose | Setup Guide |
|---|---|---|
| **Claude Desktop** | AI co-pilot + MCP hub | [`claude-desktop.md`](claude-desktop.md) |
| **GitHub** | Code repository, issues, PRs, CI/CD | [`github.md`](github.md) |
| **Google Drive** | Shared documentation workspace | [`google-drive.md`](google-drive.md) |
| **Google Calendar** | Sprint ceremonies, meeting scheduling | [`google-calendar.md`](google-calendar.md) |
| **Slack** | Team communication, alerts, standups | [`slack.md`](slack.md) |
| **Figma** | UI/UX design and design system | [`figma.md`](figma.md) |
| **Fireflies** | Meeting transcription and AI summaries | [`fireflies.md`](fireflies.md) |
| **Linear** | Sprint management, roadmap, velocity | [`linear.md`](linear.md) |
| **Sentry** | Production error and crash tracking | [`sentry.md`](sentry.md) |
| **Datadog** | Infrastructure metrics, APM, log management | [`datadog.md`](datadog.md) |

---

## Tool Matrix — Who Needs What

`✅ Required` &nbsp; `🔶 Recommended` &nbsp; `👁 Viewer access` &nbsp; `—  Not needed`

| Tool | CTO | Product Dev | UIX | PM | Frontend | Backend | Image Vision | QA/QC | DevOps | Mobile |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Claude Desktop** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **GitHub** | ✅ | ✅ | 👁 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Google Drive** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Google Calendar** | ✅ | 🔶 | 🔶 | ✅ | 🔶 | 🔶 | 🔶 | 🔶 | 🔶 | 🔶 |
| **Slack** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Figma** | 👁 | 👁 | ✅ | 👁 | 👁 | — | — | 👁 | — | 👁 |
| **Fireflies** | ✅ | ✅ | 🔶 | ✅ | 🔶 | 🔶 | 🔶 | 🔶 | 🔶 | 🔶 |
| **Linear** | ✅ | 🔶 | — | ✅ | 🔶 | 🔶 | — | 🔶 | 🔶 | 🔶 |
| **Sentry** | ✅ | — | — | — | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Datadog** | ✅ | — | — | — | — | ✅ | 🔶 | 🔶 | ✅ | — |

---

## Setup Order — What to Connect First

Connect tools in this priority order. The first four are universal and should be done on Day 1 by every team member.

### Day 1 — Universal (Everyone)

1. **Claude Desktop** — install, sign in, accept project invitation
2. **Google Drive** — install Drive Desktop, connect workspace folder in Claude
3. **GitHub** — create account, accept org invitation, connect MCP
4. **Slack** — join workspace, join project channels, connect MCP
5. **Google Calendar** — connect MCP, join shared project calendar

### Day 1 — Role-Specific

Connect these on Day 1 based on your role:

| If you are... | Also connect on Day 1 |
|---|---|
| UIX Designer | **Figma** (Editor seat — request from IT Head) |
| Project Manager | **Linear** + **Fireflies** |
| CTO / IT Head | All tools |
| Any Engineer | **Sentry** (connect before first feature goes to staging) |

### Before First Sprint

| Tool | Who | When |
|---|---|---|
| **Fireflies** | Everyone | Before your first project meeting |
| **Linear** | Engineers | Before Sprint 1 planning |
| **Sentry** | Backend, Frontend, Mobile, QA | Before first staging deployment |
| **Datadog** | Backend, DevOps | Before first infrastructure provisioning |

---

## Per-Role Quick Setup Checklist

### CTO / IT Head
- [ ] Claude Desktop — all projects accessible
- [ ] GitHub — Owner role in `qtrust-id` org
- [ ] Google Drive — all project folders accessible
- [ ] Google Calendar — MCP connected, all project calendars visible
- [ ] Slack — all project channels visible
- [ ] Figma — Admin access to QTrust org
- [ ] Fireflies — workspace admin, auto-join enabled
- [ ] Linear — Admin access to QTrust workspace
- [ ] Sentry — Admin access to QTrust org
- [ ] Datadog — Admin access to QTrust org

### Product Development
- [ ] Claude Desktop
- [ ] GitHub — Read access, can create/comment issues
- [ ] Google Drive — `docs/product/` and `by-team/product/` accessible
- [ ] Slack — joined `#[project-code]-general` and `#[project-code]-standup`
- [ ] Figma — Viewer access to design files
- [ ] Fireflies — meeting transcripts accessible

### UIX Designer
- [ ] Claude Desktop + Figma MCP verified
- [ ] Figma — **Editor seat** confirmed (contact IT Head)
- [ ] Google Drive — `designs/` folders accessible
- [ ] Slack — joined `#design-feedback`
- [ ] GitHub — Viewer, can comment on `team:uix` issues
- [ ] Fireflies

### Project Manager
- [ ] Claude Desktop
- [ ] GitHub — **Maintainer** role (can manage labels, milestones, Kanban)
- [ ] Google Drive — `by-team/pm/` and `docs/meetings/` accessible
- [ ] Google Calendar — MCP connected, shared project calendar created and shared with team
- [ ] Linear — Member with full issue management access
- [ ] Slack — all project channels
- [ ] Fireflies — required, auto-join enabled, `fred@fireflies.ai` added to all recurring ceremonies
- [ ] Figma — Viewer

### Frontend Engineer
- [ ] Claude Desktop
- [ ] GitHub — Write access, can push branches and open PRs
- [ ] Google Drive
- [ ] Slack
- [ ] Figma — Viewer + Dev Mode access
- [ ] Sentry — access to `[project-code]-frontend` project

### Backend Engineer
- [ ] Claude Desktop
- [ ] GitHub — Write access
- [ ] Google Drive
- [ ] Slack
- [ ] Sentry — access to `[project-code]-backend` project
- [ ] Datadog — access to backend service dashboards

### Image Vision Engineer
- [ ] Claude Desktop
- [ ] GitHub — Write access (vision/ML repo)
- [ ] Google Drive — `by-team/vision/` accessible
- [ ] Slack
- [ ] Sentry — access to `[project-code]-ai-serving` project

### QA / QC
- [ ] Claude Desktop
- [ ] GitHub — Write access (create/manage bug issues)
- [ ] Google Drive — `by-team/qa/` and `reports/qa/` accessible
- [ ] Slack
- [ ] Figma — Viewer (visual QA reference)
- [ ] Sentry — access to all project Sentry projects
- [ ] Linear — Read access to track issue status

### DevOps
- [ ] Claude Desktop
- [ ] GitHub — Write access (manage CI/CD workflows)
- [ ] Google Drive — `by-team/devops/` accessible
- [ ] Slack — `#[project-code]-deployments` and `#incidents`
- [ ] Sentry — Admin (creates and configures Sentry projects)
- [ ] Datadog — Admin (provisions agents, creates dashboards and alerts)
- [ ] Linear — Read access for infra tasks

### Mobile Apps Engineer
- [ ] Claude Desktop
- [ ] GitHub — Write access to mobile repository
- [ ] Google Drive
- [ ] Slack
- [ ] Figma — Viewer + Dev Mode
- [ ] Sentry — access to `[project-code]-mobile-android` and `[project-code]-mobile-ios`

---

## CTO Morning Brief — Sample Prompts

Once all tools are connected, use these prompts every morning for full project visibility:

```
"Give me a CTO morning brief across all active QTrust projects:
1. Google Calendar: what meetings and ceremonies are scheduled today?
2. Linear: sprint progress (% done) and blocked issues per project
3. GitHub: PRs waiting > 2 days for review
4. Sentry: new production errors since yesterday
5. Datadog: any monitors currently in Alert state
6. Fireflies: key decisions or action items from yesterday's meetings
7. Slack: any escalations or critical messages in #incidents or #[project]-alerts"
```

```
"Which QTrust project is most at risk right now based on:
- Blocked issues in Linear
- Open critical bugs in Sentry
- Failed CI/CD runs in GitHub
- Unresolved monitors in Datadog"
```

---

## Troubleshooting

| Problem | Solution |
|---|---|
| MCP not showing as connected | Sign out and reconnect; check OAuth permissions |
| Claude can't see project files | Verify Google Drive Desktop is synced; re-select workspace folder |
| GitHub MCP returns 403 | Ask IT Head to verify your org role and repo access |
| Linear shows wrong workspace | Disconnect and reconnect; select QTrust workspace specifically |
| Sentry returns no results | Verify you are added to the QTrust Sentry org |
| Datadog MCP fails | API key may have expired; request a new key from IT Head |

For any tool access issues, raise in `#engineering` on Slack or contact the IT Head directly.

---

*Maintained by QTrust Engineering · [`qtrust-id/ONBOARDING`](https://github.com/qtrust-id/ONBOARDING)*
