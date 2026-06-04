# Onboarding Guide — Product Development

**Team:** Product Development  
**Role Overview:** Define what gets built, why it gets built, and validate that what is built solves the right problems.

---

## 1. Welcome

You are the voice of the user and the business within the engineering team. Your work — PRDs, user stories, API specifications, and acceptance criteria — is the foundation everything else is built on. Without clear, well-reasoned product documentation, engineers build the wrong things and designers design for the wrong problems.

This guide will walk you through everything you need to get started contributing to the project from day one.

---

## 2. Your Responsibilities

As a member of the Product Development team, you own the following:

**Discovery & Definition**
- Research user needs and business requirements through interviews, surveys, and competitive analysis
- Define the problem space clearly before proposing solutions
- Prioritize features using frameworks such as MoSCoW, RICE, or impact/effort matrices

**Documentation**
- Write Product Requirements Documents (PRDs) for every major feature or module
- Author user stories with clear acceptance criteria in the Given/When/Then format
- Maintain a living product changelog that records all requirement changes and their rationale

**Collaboration**
- Work closely with UIX Designers to ensure designs address the documented requirements
- Partner with Backend Engineers to define API contracts before development begins
- Coordinate with the Project Manager to translate approved requirements into sprint-ready GitHub Issues
- Participate in sprint planning, sprint reviews, and retrospectives

**Validation**
- Review completed features against acceptance criteria before QA testing begins
- Conduct or facilitate User Acceptance Testing (UAT) on the **staging** environment before any production release — sign-off on staging is the gate to release (see [`../environments-and-promotion.md`](../environments-and-promotion.md))
- Gather post-launch feedback and feed learnings back into future requirements

---

## 3. Tools & Access Setup

Complete these steps on your first day.

### 3.1 Google Drive
- Request access to the shared **project folder** from your team lead
- Install **Google Drive Desktop** on your machine and sign in with your `@qtrust.id` Google Workspace account
- Confirm the project folder appears at: `Google Drive > Shared drives > QTrust > [PROJECT_CODE]`
- Your primary working directory is `[PROJECT_CODE]/docs/product/` and `[PROJECT_CODE]/by-team/product/`

### 3.2 GitHub
- Create a GitHub account if you do not have one, using your `@qtrust.id` email
- Request to be added to the `qtrust-id` GitHub organization with the **Product** role (read/comment access)
- Familiarize yourself with the repository: the project repository (URL from Project Config Sheet)
- You will primarily read issues and create/comment on issues — you do not need to push code

### 3.3 Claude Desktop
- Install Claude Desktop from [claude.ai/download](https://claude.ai/download)
- Sign in with your QTrust premium account (credentials from your team lead)
- Connect your Google Drive workspace folder: Settings → Workspace → point to your local `[PROJECT_CODE]/` path
- Claude is your primary writing assistant. Use it to draft PRDs, user stories, competitive analysis, API specs, and meeting notes

### 3.4 Figma
- Request view access to the project Figma workspace (link in `by-team/uix/figma-links.md`)
- You do not need an editor seat; view-only access is sufficient for reviewing and commenting on designs

---

## 4. Project Structure You Own

```
[PROJECT_CODE]/
├── docs/
│   ├── product/               ← Your primary output folder
│   │   ├── PRD-[module-a].md
│   │   ├── PRD-[module-b].md
│   │   ├── PRD-[module-c].md
│   │   ├── PRD-[module-d].md
│   │   ├── PRD-[module-e].md
│   │   ├── PRD-[module-f].md
│   │   ├── user-stories/
│   │   └── changelog.md
│   └── api/                   ← Co-own with Backend
│       └── openapi.yaml
└── by-team/
    └── product/               ← Working drafts, research, notes
        ├── competitive-analysis.md
        ├── user-personas.md
        ├── feature-priority.md
        └── roadmap.md
```

---

## 5. PRD Format & Standards

Every PRD must follow this structure. Use Claude to generate an initial draft, then refine it.

```markdown
# PRD — [Module Name]

**Status:** Draft | In Review | Approved | Deprecated  
**Author:** [Your Name]  
**Reviewers:** [Names]  
**Last Updated:** YYYY-MM-DD  
**GitHub Milestone:** [link]

---

## 1. Background & Problem Statement
Describe the current pain point and why it matters to users and the business.

## 2. Goals
- What success looks like (measurable outcomes)
- What this PRD is NOT trying to solve

## 3. User Personas
Who will use this feature and what are their contexts?

## 4. User Stories
As a [role], I want to [action] so that [outcome].

### Acceptance Criteria
Given [context], when [action], then [expected result].

## 5. Functional Requirements
Numbered list of specific behaviors the system must exhibit.

## 6. Non-Functional Requirements
Performance, security, accessibility, and compliance requirements.

## 7. API Contract (Draft)
High-level overview of endpoints needed. Detailed spec in docs/api/.

## 8. UI/UX Notes
Key interaction patterns and constraints for the UIX Designer.

## 9. Out of Scope
Explicitly list what will not be addressed in this version.

## 10. Open Questions
Unresolved issues that must be answered before development begins.

## 11. Dependencies
Other modules, third-party services, or team deliverables this relies on.
```

---

## 6. User Story Format

Use the Given/When/Then format consistently.

**Good example:**
```
As a [User Role], I want to export the [Report Type] as a document
so that I can submit it to the relevant team without reformatting.

Acceptance Criteria:
- Given I am logged in as [User Role],
  When I navigate to [MODULE_NAME] > Reports and click "Export",
  Then the system generates a document containing all relevant records
  for the selected period, formatted with QTrust branding, within 5 seconds.

- Given the report contains more than 500 rows,
  When the export is triggered,
  Then the system processes it as a background job and sends the download
  link via email when ready.
```

---

## 7. Workflow

```
1. Discovery
   ↓ Research, interviews, competitive analysis
   ↓ Document findings in by-team/product/

2. Problem Definition
   ↓ Write problem statement
   ↓ Get alignment from CTO and PM

3. PRD Draft (Claude-assisted)
   ↓ Use Claude: "Write a PRD for the [MODULE_NAME] of [application type]"
   ↓ Fill in business-specific details
   ↓ Save to docs/product/PRD-[module].md

4. PRD Review
   ↓ Share Google Drive link with PM, Lead Engineer, UIX Designer
   ↓ Collect feedback via Google Drive comments
   ↓ Revise until status = "Approved"

5. Handoff
   ↓ Notify PM to convert user stories to GitHub Issues
   ↓ Brief UIX Designer on UX requirements
   ↓ Brief Backend on API contract needs

6. Development Support
   ↓ Answer clarification questions from engineers during development
   ↓ Review implemented features against acceptance criteria

7. UAT & Launch
   ↓ Facilitate UAT with real users or stakeholders
   ↓ Sign off on release
   ↓ Document post-launch feedback
```

---

## 8. Collaboration With Other Teams

| Team | How You Work Together |
|---|---|
| **UIX Designer** | Share approved PRDs before design begins. Review wireframes and hi-fi mockups against requirements. Leave comments in Figma. |
| **Project Manager** | Approved PRDs become the source for GitHub Issues. Attend sprint planning to clarify requirements. |
| **Backend Engineer** | Draft API contracts collaboratively. Confirm data models match business requirements. |
| **Frontend Engineer** | Clarify edge cases in user stories during development. Review completed UI against acceptance criteria. |
| **QA / QC** | Share acceptance criteria before testing begins. Review QA test plans to ensure coverage is complete. |

---

## 9. Working with Claude Desktop

Claude is your most powerful productivity tool on this project. Here are the most effective prompts for your role:

**Drafting a PRD:**
> "Write a comprehensive PRD for the [MODULE_NAME] module of this application. The system must handle [domain-specific calculation]. Use the standard PRD template."

**Writing user stories:**
> "Convert these requirements into user stories with Given/When/Then acceptance criteria: [paste requirements]"

**Competitive analysis:**
> "Analyze the key features of [Competitor A / Competitor B / Competitor C] and identify gaps that a new product could address."

**API contract draft:**
> "Draft a high-level REST API contract for a [MODULE_NAME] module. Include endpoints for the primary create, read, update, and delete operations."

**Meeting notes (Fireflies MCP):**
> "Get the Fireflies transcript of the [meeting] on [date] and turn it into structured minutes with a decisions section and an action items table." (Fireflies auto-transcribes project meetings — see [`tools/fireflies.md`](../tools/fireflies.md) — so you rarely need to paste a transcript manually.)

Always review Claude's output critically. Claude drafts; you decide.

---

## 10. Git & GitHub Usage

As a Product team member, your GitHub usage is primarily issue-based rather than code-based.

- **Read** the repository to understand implementation progress
- **Create issues** when you identify missing requirements or scope changes (use label `type:requirement-change`)
- **Comment** on issues to provide context or clarification
- **Link PRDs** to milestones: every GitHub Milestone should have a corresponding PRD in `docs/product/`

---

## 11. First Week Checklist

- [ ] Google Drive access confirmed and folder synced locally
- [ ] GitHub account created and added to `qtrust-id` organization
- [ ] Claude Desktop installed, signed in, workspace folder connected
- [ ] Figma view access granted
- [ ] Read all existing PRDs in `docs/product/`
- [ ] Read `docs/architecture/system-design.md`
- [ ] Review `by-team/pm/roadmap.md` to understand sprint schedule
- [ ] Attended kickoff meeting and sprint planning
- [ ] First PRD draft started (assign yourself a module from the backlog)
- [ ] Introduced yourself in the team channel

---

## 12. Key Resources

| Resource | Location |
|---|---|
| All PRDs | `docs/product/` |
| API Specifications | `docs/api/` |
| System Architecture | `docs/architecture/` |
| Sprint Roadmap | `by-team/pm/roadmap.md` |
| Risk Register | `by-team/pm/risk-register.md` |
| Figma Designs | `by-team/uix/figma-links.md` |
| GitHub Repository | `src-link/GITHUB.md` |
| Onboarding Hub | `docs/onboarding/README.md` |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (read) · Google Drive · Slack · Figma (viewer) · Fireflies

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Fireflies | [`tools/fireflies.md`](../tools/fireflies.md) | Before first meeting |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
