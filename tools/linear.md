# Tool Setup Guide — Linear MCP

> **Required for:** CTO, Project Manager, Team Leads
> **Recommended for:** All engineers (read access for issue tracking)
> **Role:** Primary project and sprint management platform. MCP integration lets Claude create issues, track velocity, surface blockers, and generate project status reports directly from conversations.

---

## 1. What It Is

Linear is QTrust's project management platform — used for sprint planning, issue tracking, roadmaps, and team workload management. It is a significant upgrade over GitHub Projects for CTO and PM visibility: Linear offers cycle (sprint) tracking, team velocity charts, priority management, and a clean roadmap view across multiple projects simultaneously.

The Linear MCP integration means Claude can query and update Linear data without you opening a browser. As CTO, you can ask Claude for a cross-project status report in seconds.

---

## 2. Who Needs This

| Role | Access | Primary Usage |
|---|---|---|
| CTO / IT Head | Admin | Cross-project roadmap, team health, velocity |
| Project Manager | Member | Sprint planning, issue management, blockers |
| Team Leads | Member | Team workload, cycle progress |
| All Engineers | Member (read + write own issues) | View assigned issues, update status |
| QA / QC | Member | Track bugs and test coverage issues |

---

## 3. How Linear Relates to GitHub Issues

QTrust uses **both** Linear and GitHub Issues. They serve different purposes:

| GitHub Issues | Linear |
|---|---|
| Linked to specific code commits and PRs | Linked to project cycles (sprints) and roadmap |
| Engineers update during development | PM and leads use for planning and reporting |
| CI/CD status, code reviews | Velocity, burndown, team workload |
| Technical bug reports | Strategic priorities and deadlines |

In practice: GitHub Issues are the source of truth for code work. Linear is the source of truth for project health visible to leadership.

---

## 4. Workspace Setup

1. Go to [linear.app](https://linear.app) and sign up with your `@qtrust.id` Google account
2. Ask the IT Head to invite you to the QTrust Linear workspace
3. Accept the invitation email
4. You will see the workspace with projects for each active QTrust project

---

## 5. Linear Structure — QTrust Convention

```
QTrust Workspace
├── Team: [PROJECT_CODE] (e.g., QT-PORTAL)
│   ├── Cycles (Sprints)
│   │   ├── Sprint 0 — Setup
│   │   ├── Sprint 1 — [Module A]
│   │   └── ...
│   ├── Backlog
│   ├── Roadmap
│   └── Members
└── Team: [PROJECT_CODE 2]
    └── ...
```

---

## 6. Issue Status Workflow

All issues move through these statuses:

```
Backlog → Todo → In Progress → In Review → Done
                                    ↓
                              Cancelled (if dropped)
```

| Status | Who Sets It | Meaning |
|---|---|---|
| **Backlog** | PM | Accepted but not yet in a sprint |
| **Todo** | PM (sprint planning) | Committed for current sprint |
| **In Progress** | Engineer | Actively being worked on |
| **In Review** | Engineer | PR open, awaiting code review |
| **Done** | Engineer / QA | Merged and QA passed |

---

## 7. Connecting Linear MCP in Claude Desktop

1. Open Claude Desktop → **Settings** → **Integrations**
2. Find **Linear** and click **Connect**
3. You will be redirected to Linear to authorise Claude's access
4. Select the QTrust workspace and grant the requested permissions
5. Return to Claude Desktop — the connection should show **Connected**

---

## 8. Verification Test

Run these prompts in Claude after connecting to confirm the integration is working:

```
"List all open issues in the [PROJECT_CODE] Linear team."
"What issues are In Progress for the current sprint?"
"Show me the backlog prioritised by urgency."
```

---

## 9. Usage Examples by Role

**CTO — Morning brief:**
```
"Give me a status report across all active Linear projects:
- Sprint progress (% done) per project
- Any issues marked as blocked
- Issues that haven't moved status in 3+ days
- Upcoming cycle end dates"
```

**Project Manager — Sprint planning:**
```
"Create Linear issues for Sprint [N] based on these user stories: [paste stories].
For each issue: title, description, estimate (story points), assignee (if known), and labels."
```

**Project Manager — Blocker escalation:**
```
"List all Linear issues with status 'In Progress' that have been in that status for more than 5 days.
Group by team member. This helps identify where people are stuck."
```

**Engineer:**
```
"Show me all Linear issues assigned to me in the current [PROJECT_CODE] sprint."
"Move issue [ID] to In Review status and add a comment: 'PR #[number] open for review.'"
```

**QA / QC:**
```
"List all issues in Linear tagged as bug for the current sprint.
Which ones are still open? Which are marked Done but haven't been verified?"
```

---

## 10. First-Day Checklist

- [ ] Linear account created with `@qtrust.id` Google SSO
- [ ] Added to QTrust Linear workspace with correct role
- [ ] Can see all active project teams in the workspace
- [ ] Linear MCP connected in Claude Desktop (run verification test)
- [ ] First sprint issues visible via Claude query
