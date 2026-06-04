# Onboarding Guide — Project Manager

**Team:** Project Management  
**Role Overview:** Keep the project on track — on time, within scope, and with every team member unblocked and aligned.

---

## 1. Welcome

You are the connective tissue of this project. While other teams focus on their specific disciplines, you maintain the whole picture: what needs to be done, who is doing it, when it is due, and what is in the way. Your success is measured not by what you personally produce, but by how well the entire team delivers.

This guide covers your tools, processes, sprint cadence, and how to use Claude Desktop to run a highly organized, low-friction project.

---

## 2. Your Responsibilities

**Planning**
- Translate approved PRDs from Product Development into sprint-ready GitHub Issues
- Create and maintain milestones, labels, and the sprint schedule in GitHub
- Own the project roadmap and keep it current in `by-team/pm/roadmap.md`
- Facilitate sprint planning meetings and ensure all team members leave with a clear sprint goal

**Tracking & Communication**
- Monitor sprint progress daily; identify blockers before they become delays
- Run daily standups (async or synchronous, as the team prefers)
- Maintain the risk register in `by-team/pm/risk-register.md`
- Report project status to stakeholders on a weekly basis

**Process & Ceremonies**
- Sprint Planning (every 2 weeks, start of sprint)
- Daily Standup (every day — 15 minutes max)
- Sprint Review (every 2 weeks, end of sprint — demo to stakeholders)
- Sprint Retrospective (every 2 weeks, immediately after review)
- Backlog Grooming (mid-sprint, 1 hour, with Product and Tech Leads)

**Documentation**
- Record all meeting notes in `docs/meetings/` using the standard template
- Maintain a decision log so context is never lost when team members change
- Ensure every sprint plan is archived in `by-team/pm/`

---

## 3. Tools & Access Setup

### 3.1 GitHub
- Create or log in to your GitHub account using your `@qtrust.id` email
- Request **Maintainer** role in the `qtrust-id` organization — you need permission to create and manage Issues, Milestones, and Labels
- Bookmark the project repository (URL from Project Config Sheet)
- Familiarize yourself with the Issues board, the Projects tab (Kanban), and the Milestones section

### 3.2 Claude Desktop
- Install Claude Desktop and sign in with your QTrust premium account
- Connect your local `[PROJECT_CODE]/` Google Drive folder as the workspace
- Claude will help you break down PRDs into issues, draft sprint plans, write risk assessments, and generate meeting notes
- Key workflow: paste a PRD into Claude and ask it to generate a full set of GitHub Issues with titles, descriptions, labels, and story point estimates

### 3.3 Google Drive
- Confirm access to the shared project Google Drive folder
- Your primary folders:
  - `by-team/pm/` — sprint plans, roadmap, risk register, stakeholder map
  - `docs/meetings/` — all meeting notes
  - `reports/sprint/` — retrospectives and velocity reports

### 3.4 Linear
- Linear is QTrust's **primary sprint and project management platform** — request **Member** access to the QTrust Linear workspace from the IT Head
- You own the project's Linear team (`[PROJECT_CODE]`): cycles (sprints), backlog, and roadmap
- Connect the Linear MCP in Claude Desktop so you can create issues, track velocity, and surface blockers directly from a conversation (see [`tools/linear.md`](../tools/linear.md))
- See Section 4.4 for how Linear and GitHub Issues divide responsibilities

### 3.5 Google Calendar
- Connect the Google Calendar MCP in Claude Desktop (see [`tools/google-calendar.md`](../tools/google-calendar.md))
- Ask the IT Head to create the shared `[PROJECT_CODE] — Team` calendar and share it with the whole team
- You schedule **all** sprint ceremonies on this shared calendar — Claude can create the full recurring set in one prompt (see Section 8)

### 3.6 Fireflies
- Connect the Fireflies MCP and enable **auto-join** for your `@qtrust.id` calendar (see [`tools/fireflies.md`](../tools/fireflies.md))
- Add `fred@fireflies.ai` to every recurring ceremony so standups, planning, reviews, and retros are transcribed automatically
- After each meeting, use Claude to extract decisions, action items, and blockers from the Fireflies transcript instead of taking notes manually

### 3.7 Figma (View Only)
- Request view access to the project Figma workspace
- You use Figma to understand design progress and verify that designs are available before frontend sprints begin

---

## 4. GitHub & Linear Setup

You are responsible for the full project management setup across both GitHub and Linear. Complete this before Sprint 1.

> **Two tools, two purposes.** QTrust uses **both** Linear and GitHub Issues — they are not redundant. GitHub Issues stay tightly coupled to code (commits, PRs, CI, branch references) and are the source of truth for engineering work. Linear is the source of truth for **project health** — cycles, velocity, roadmap, and the leadership view. As PM you keep both in sync: technical issues live in GitHub, while the sprint plan, priorities, and reporting live in Linear. See Section 4.4 and [`tools/linear.md`](../tools/linear.md).

### 4.1 Labels to Create
Create the following labels in the repository:

**Module labels** (color: purple shades)  
Create one label per module defined in the Project Config Sheet (e.g., `module:[module-name]`).

**Type labels** (color: green/red/grey)
- `type:feature`
- `type:bug`
- `type:chore`
- `type:documentation`
- `type:question`
- `type:requirement-change`

**Priority labels** (color: red/orange/yellow)
- `priority:critical`
- `priority:high`
- `priority:medium`
- `priority:low`

**Team labels** (color: blue shades)
- `team:product`
- `team:uix`
- `team:frontend`
- `team:backend`
- `team:qa`
- `team:devops`
- `team:mobile`

**Status labels**
- `status:blocked`
- `status:needs-design`
- `status:needs-review`
- `status:ready-for-qa`

### 4.2 Milestones to Create

Define milestones based on the modules listed in the Project Config Sheet. The example below is a template — replace sprint names and descriptions with your project's actual modules and schedule.

| Milestone | Description | Due Date |
|---|---|---|
| Sprint 0 — Setup | Project scaffolding, design system, env setup | Week 1 |
| Sprint 1 — [Module A] | [Module A] complete | Week 3 |
| Sprint 2 — [Module B] | [Module B] complete | Week 5 |
| Sprint 3 — [Module C] | [Module C] complete | Week 7 |
| Sprint N — UAT & Deploy | Bug fixing, UAT, production deployment | Week N+1 |

### 4.3 GitHub Projects (Kanban Board)
Create a GitHub Project with the following columns:
1. **Backlog** — accepted but not yet in a sprint
2. **Sprint Backlog** — committed for current sprint
3. **In Progress** — actively being worked on
4. **In Review** — PR open, awaiting code review
5. **Ready for QA** — merged to develop, awaiting QA testing
6. **Done** — QA passed, merged to main or sprint-complete

> The GitHub Projects Kanban gives engineers a code-centric board. Linear cycles (Section 4.4) give you and leadership the sprint, velocity, and roadmap view. Mirror status between them: the Linear statuses map directly to these columns.

### 4.4 Linear Cycles & Roadmap

In the QTrust Linear workspace, create a team named `[PROJECT_CODE]` and set it up before Sprint 1:

- **Cycles** — one Linear cycle per sprint (`Sprint 0 — Setup`, `Sprint 1 — [Module A]`, …), matching the GitHub Milestones
- **Backlog** — accepted work not yet committed to a cycle
- **Roadmap** — the multi-sprint view you share with the CTO and stakeholders
- **Issue status flow:** `Backlog → Todo → In Progress → In Review → Done` (mirrors the Kanban columns)

Use Claude with the Linear MCP to create a whole sprint's issues from approved user stories in one prompt (see Section 8). Keep GitHub Issues for the technical detail and link them from the corresponding Linear issue.

---

## 5. Issue Format Standard

Every GitHub Issue created from a PRD must follow this format:

```markdown
## Summary
[One sentence describing what needs to be built]

## Context
[Why this is needed — link to the PRD section]
PRD: [link to Google Drive PRD]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

## Technical Notes
[Any technical context the engineer needs — optional]

## Dependencies
- Blocked by: #[issue number] (if applicable)
- Designs available: [Yes / No — Figma link]

## Estimation
Story Points: [1 / 2 / 3 / 5 / 8 / 13]
```

---

## 6. Sprint Workflow

> **Schedule once, capture automatically.** At the start of each sprint, schedule every ceremony below on the shared `[PROJECT_CODE] — Team` Google Calendar (Claude can create the full recurring set in one prompt — see Section 8) and ensure `fred@fireflies.ai` is invited to each. Fireflies then transcribes every ceremony, so your notes, action items, and blocker tracking come from the transcript rather than manual minute-taking.

### Sprint Planning (Day 1 of Sprint)
1. Review the sprint backlog — ensure all issues are correctly estimated and have designs attached (if needed)
2. Team commits to a sprint goal
3. Issues are moved from Backlog → Sprint Backlog in the Kanban board
4. Each issue is assigned to a team member

### Daily Standup (Every Day, 15 min max)
Format:
- What did you complete yesterday?
- What are you working on today?
- Are you blocked by anything?

As PM, your job is to identify blockers and resolve them — not to simply record them. After each standup, ask Claude to pull the Fireflies transcript and extract every mention of "blocked", "blocker", or "waiting on", grouped by owner — this surfaces stuck work without you transcribing the call.

### Mid-Sprint Grooming (Day 5 of Sprint)
- Review backlog with Product Development and Tech Leads
- Refine, estimate, and prioritize issues for the next sprint
- Ensure designs are ready for features planned 1 sprint ahead

### Sprint Review (Last Day of Sprint)
- Team demos completed work to stakeholders
- Only work that meets the Definition of Done is presented
- Collect stakeholder feedback and create follow-up issues if needed

### Sprint Retrospective (After Review, 45 min)
- What went well? What needs improvement? What actions will we take?
- Document results in `reports/sprint/sprint-[n]-retro.md`
- Track action items from previous retrospective

---

## 7. Definition of Done

A story is **Done** when all of the following are true:
- [ ] Code is written and passes all automated tests
- [ ] PR is reviewed and approved by at least one other engineer
- [ ] Merged to `develop` and auto-deployed to the **staging** environment
- [ ] QA has tested on staging against acceptance criteria — no critical/high bugs open
- [ ] Feature matches the Figma design (UIX sign-off)
- [ ] Relevant documentation is updated
- [ ] GitHub Issue is closed

> Production release happens separately, by promoting `develop` → `main` with approval once a sprint's work is signed off on staging. See [`../environments-and-promotion.md`](../environments-and-promotion.md).

---

## 8. Working with Claude Desktop

**Break down a PRD into GitHub Issues:**
> "Read this PRD and generate a full list of GitHub Issues. For each issue, include: title (imperative verb), description, acceptance criteria (checkbox list), suggested labels, and story point estimate (Fibonacci scale). PRD: [paste content]"

**Write a sprint plan:**
> "Based on these GitHub Issues and a 2-week sprint with 4 developers, create a sprint plan document. Assign issues to days and flag any dependencies or risks. Issues: [paste list]"

**Draft meeting notes:**
> "Turn this standup into a structured record with a blockers section and action items: [paste notes]"

**Risk assessment:**
> "Review this project plan and identify the top 5 risks. For each risk, assess probability (high/medium/low), impact (high/medium/low), and suggest a mitigation strategy."

**Status report:**
> "Write a weekly status update email to stakeholders. Tone: professional, concise. Status: [describe progress, blockers, next steps]"

**Create a sprint's issues in Linear (Linear MCP):**
> "Create Linear issues for Sprint [N] in the [PROJECT_CODE] team based on these user stories: [paste]. For each issue: title, description, story-point estimate, labels, and assignee if known. Place them in the current cycle."

**Sprint health check (Linear MCP):**
> "In Linear, show me sprint progress (% done) for the [PROJECT_CODE] current cycle, any issues marked blocked, and any issue that hasn't changed status in 3+ days."

**Schedule all ceremonies at once (Google Calendar MCP):**
> "Schedule all Sprint [N] ceremonies on the [PROJECT_CODE] — Team calendar from [start] to [end]: daily standup (Mon–Fri 09:30–09:45), sprint planning, backlog grooming, sprint review, and retro. Invite [team emails] and `fred@fireflies.ai`, and add an agenda description to each."

**Extract action items from a meeting (Fireflies MCP):**
> "Get the Fireflies transcript of the sprint planning on [date]. Extract all action items with the responsible person and any mentioned deadlines, and list the agreed sprint goal."

---

## 9. Stakeholder Communication

**Weekly Status Report** — sent every Friday to relevant stakeholders. Format:

```
Subject: [PROJECT_NAME] Weekly Update — [Week N] — [Date]

Summary: [1-2 sentences on overall health — on track / at risk / blocked]

Completed this week:
- [Feature/milestone completed]

In progress:
- [What the team is working on]

Upcoming (next week):
- [Next sprint priorities]

Risks & Issues:
- [Any active risks — reference risk register]

Questions / Decisions needed:
- [If you need stakeholder input]
```

---

## 10. First Week Checklist

- [ ] GitHub account with Maintainer access confirmed
- [ ] All labels created in the repository
- [ ] All milestones created with due dates
- [ ] GitHub Projects Kanban board set up
- [ ] Linear team created with cycles matching the sprint milestones; Linear MCP connected
- [ ] Shared `[PROJECT_CODE] — Team` Google Calendar created; all Sprint 1 ceremonies scheduled; Calendar MCP connected
- [ ] Fireflies auto-join enabled and `fred@fireflies.ai` invited to all recurring ceremonies
- [ ] Claude Desktop installed and workspace folder connected
- [ ] Google Drive folder synced locally
- [ ] All existing PRDs read — understand scope
- [ ] `by-team/pm/roadmap.md` created or updated
- [ ] `by-team/pm/risk-register.md` reviewed and updated
- [ ] Sprint 0 issues created and assigned
- [ ] Kickoff meeting facilitated and notes recorded in `docs/meetings/`
- [ ] All team members confirmed their tool access

---

## 11. Key Resources

| Resource | Location |
|---|---|
| Project Roadmap | `by-team/pm/roadmap.md` |
| Risk Register | `by-team/pm/risk-register.md` |
| Meeting Notes | `docs/meetings/` |
| Sprint Retrospectives | `reports/sprint/` |
| All PRDs | `docs/product/` |
| GitHub Repository | `src-link/GITHUB.md` |
| Branch Strategy | `src-link/GITHUB.md` |
| Environments & Promotion | [`environments-and-promotion.md`](../environments-and-promotion.md) (this repo) |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (maintainer) · Google Drive · Google Calendar · Linear · Slack · Fireflies · Figma (viewer)

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| Google Calendar | [`tools/google-calendar.md`](../tools/google-calendar.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Figma | [`tools/figma.md`](../tools/figma.md) | Day 1 |
| Fireflies | [`tools/fireflies.md`](../tools/fireflies.md) | Before first meeting |
| Linear | [`tools/linear.md`](../tools/linear.md) | Before Sprint 1 |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
