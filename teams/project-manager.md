# Onboarding Guide — Project Manager

**Team:** Project Management  
**Role in HRIS:** Keep the project on track — on time, within scope, and with every team member unblocked and aligned.

---

## 1. Welcome

You are the connective tissue of the HRIS project. While other teams focus on their specific disciplines, you maintain the whole picture: what needs to be done, who is doing it, when it is due, and what is in the way. Your success is measured not by what you personally produce, but by how well the entire team delivers.

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
- Request **Maintainer** role in the `qtrust` organization — you need permission to create and manage Issues, Milestones, and Labels
- Bookmark the repository: `github.com/qtrust/hris-app`
- Familiarize yourself with the Issues board, the Projects tab (Kanban), and the Milestones section

### 3.2 Claude Desktop
- Install Claude Desktop and sign in with your QTrust premium account
- Connect your local `QTrust/HRIS/` Google Drive folder as the workspace
- Claude will help you break down PRDs into issues, draft sprint plans, write risk assessments, and generate meeting notes
- Key workflow: paste a PRD into Claude and ask it to generate a full set of GitHub Issues with titles, descriptions, labels, and story point estimates

### 3.3 Google Drive
- Confirm access to the shared HRIS Google Drive folder
- Your primary folders:
  - `by-team/pm/` — sprint plans, roadmap, risk register, stakeholder map
  - `docs/meetings/` — all meeting notes
  - `reports/sprint/` — retrospectives and velocity reports

### 3.4 Figma (View Only)
- Request view access to the HRIS Figma workspace
- You use Figma to understand design progress and verify that designs are available before frontend sprints begin

---

## 4. GitHub Setup

You are responsible for the full GitHub project management setup. Complete this before Sprint 1.

### 4.1 Labels to Create
Create the following labels in the repository:

**Module labels** (color: purple shades)
- `module:employee`
- `module:attendance`
- `module:leave`
- `module:payroll`
- `module:performance`
- `module:recruitment`
- `module:org-chart`
- `module:mobile`

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

| Milestone | Description | Due Date |
|---|---|---|
| Sprint 0 — Setup | Project scaffolding, design system, env setup | Week 1 |
| Sprint 1 — Employee | Employee Management module complete | Week 3 |
| Sprint 2 — Attendance | Attendance module complete | Week 5 |
| Sprint 3 — Leave | Leave Management module complete | Week 7 |
| Sprint 4 — Payroll | Payroll module complete | Week 9 |
| Sprint 5 — Performance & Recruitment | Both modules complete | Week 11 |
| Sprint 6 — UAT & Deploy | Bug fixing, UAT, production deployment | Week 12 |

### 4.3 GitHub Projects (Kanban Board)
Create a GitHub Project with the following columns:
1. **Backlog** — accepted but not yet in a sprint
2. **Sprint Backlog** — committed for current sprint
3. **In Progress** — actively being worked on
4. **In Review** — PR open, awaiting code review
5. **Ready for QA** — merged to develop, awaiting QA testing
6. **Done** — QA passed, merged to main or sprint-complete

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

As PM, your job is to identify blockers and resolve them — not to simply record them.

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
- [ ] Merged to the `develop` branch
- [ ] QA has tested against acceptance criteria — no critical bugs open
- [ ] Feature matches the Figma design (UIX sign-off)
- [ ] Relevant documentation is updated
- [ ] GitHub Issue is closed

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

---

## 9. Stakeholder Communication

**Weekly Status Report** — sent every Friday to relevant stakeholders. Format:

```
Subject: HRIS Weekly Update — [Week N] — [Date]

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
