# Tool Setup Guide — GitHub MCP

> **Required for:** All team members
> **Role:** Access issues, pull requests, repositories, and CI/CD status directly from Claude conversations.

---

## 1. What It Is

The GitHub MCP integration connects Claude Desktop to the QTrust GitHub organisation (`qtrust-id`). Once connected, you can ask Claude to create issues, review PR comments, check CI/CD status, search code, and manage labels — all without opening a browser.

This is the backbone of the development workflow for all QTrust teams. Every feature, bug, and task is tracked in GitHub Issues, and Claude can query and update them in real time.

---

## 2. Who Needs This

All team members. Access level varies by role.

| Role | GitHub Role | Primary Actions in GitHub |
|---|---|---|
| CTO / IT Head | Owner | Full access — repo settings, org management, billing |
| Product Development | Member (Read) | Create issues, comment on issues, link PRDs |
| UIX Designer | Member (Read) | Read issues tagged `team:uix`, add Figma links in comments |
| Project Manager | Maintainer | Create/manage issues, labels, milestones, project Kanban board |
| Frontend Engineer | Member (Write) | Push feature branches, open PRs, merge to `develop` |
| Backend Engineer | Member (Write) | Push feature branches, open PRs, merge to `develop` |
| Image Vision Engineer | Member (Write) | Push feature branches, open PRs to assigned repo |
| QA / QC | Member (Write) | Create bug report issues, close verified issues |
| DevOps | Member (Write) | Manage CI/CD workflows, configure repository secrets |
| Mobile Apps Engineer | Member (Write) | Push branches to mobile repo, open PRs |

---

## 3. Account Setup

1. Create a GitHub account at [github.com](https://github.com) using your `@qtrust.id` email address.
   - If you already have a personal GitHub account, you can use it — but you must add your `@qtrust.id` email to it and set it as primary.
2. Ask the IT Head to add your GitHub username to the `qtrust-id` organisation with the correct role (refer to the table above).
3. Accept the organisation invitation — it will be sent to your `@qtrust.id` email inbox.
4. Confirm membership: visit [github.com/qtrust-id](https://github.com/qtrust-id) — you should see the organisation page and repositories.

---

## 4. Setting Up SSH or HTTPS Authentication

You must authenticate with GitHub when pushing code from your local machine.

### Option A: SSH (Recommended)

```bash
# Step 1: Generate an SSH key (skip if you already have one)
ssh-keygen -t ed25519 -C "your@qtrust.id"

# Step 2: Copy the public key to your clipboard
# macOS:
cat ~/.ssh/id_ed25519.pub | pbcopy
# Windows (Git Bash):
cat ~/.ssh/id_ed25519.pub | clip

# Step 3: Add it to GitHub
# Go to: github.com → Settings → SSH and GPG keys → New SSH key
# Paste the key, give it a title (e.g., "MacBook Work"), and save.

# Step 4: Test the connection
ssh -T git@github.com
# Expected output: "Hi [username]! You've successfully authenticated..."
```

### Option B: HTTPS with a Personal Access Token (PAT)

```bash
# Step 1: Generate a PAT
# Go to: github.com → Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens
# Set expiry, select qtrust-id organisation, and grant: Contents (read/write), Issues (read/write), Pull Requests (read/write)

# Step 2: Use the token as your password when Git prompts for credentials
# On macOS, store it in the Keychain to avoid re-entering it each time
```

---

## 5. Cloning Your Repository

Once your account is in the organisation, clone your assigned project repository:

```bash
# SSH (recommended):
git clone git@github.com:qtrust-id/[PROJECT_CODE]-app.git

# HTTPS:
git clone https://github.com/qtrust-id/[PROJECT_CODE]-app.git

# Move into the project directory:
cd [PROJECT_CODE]-app
```

Replace `[PROJECT_CODE]` with the code from your Project Configuration Sheet (e.g., `QT-PORTAL`, `QT-SCT`).

---

## 6. Connecting the GitHub MCP in Claude Desktop

GitHub is **not** in Claude's one-click connector directory, and its remote MCP server **cannot** be added as a custom connector in Claude Desktop yet (it requires OAuth through a registered GitHub App, which Claude Desktop doesn't support). The reliable method — and the one QTrust uses — is to run GitHub's **official MCP server locally via Docker, authenticated with a Personal Access Token (PAT)**.

You only do this once. Follow the steps in order — no prior MCP experience needed. Based on the [official GitHub guide](https://github.com/github/github-mcp-server/blob/main/docs/installation-guides/install-claude.md).

### Prerequisites

- Claude Desktop (latest version)
- **Docker Desktop** installed and **running**
- A **GitHub Personal Access Token** (created in Step 1)

### Step 1 — Create a Personal Access Token (PAT)

1. Go to **github.com → Settings → Developer settings → Personal access tokens**.
2. Simplest: create a **classic** token and tick the **`repo`** scope.
   - (Or a **fine-grained** token scoped to the `qtrust-id` org with: Contents R/W, Issues R/W, Pull requests R/W, Metadata R.)
3. **Copy the token now** — GitHub shows it only once. Treat it like a password.

### Step 2 — Open the Claude Desktop config file

In Claude Desktop: **Settings → Developer → Edit Config**. This opens `claude_desktop_config.json`. (Or open it directly:)

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

### Step 3 — Add the GitHub MCP server

Paste this block. If `mcpServers` already exists, add only the inner `github` entry. Replace `YOUR_GITHUB_PAT` with the token from Step 1:

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "ghcr.io/github/github-mcp-server"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_PAT" }
    }
  }
}
```

### Step 4 — Save and restart

Save the file and **fully quit and reopen Claude Desktop**. On first launch Docker pulls the image once (give it a moment).

### Step 5 — Verify

Run the prompts in Section 7. If Claude can list your issues, the MCP is working.

### No Docker? — `gh` CLI fallback

The **GitHub CLI (`gh`)** + `git` cover creating and managing issues, PRs, branches, and CI status without any MCP. Run `gh auth login` once; Claude can then compose `gh` commands for you.

> **Troubleshooting:** *"Authentication failed"* → the PAT lacks the `repo` scope or has expired. *"Server not starting / no GitHub tools"* → Docker Desktop isn't running, or the JSON has a syntax error (check commas and brackets). Logs on macOS: `cat ~/Library/Logs/Claude/mcp-server-*.log`.

---

## 7. Verification Test

Once connected, run these prompts in a Claude conversation:

```
"List the open issues in qtrust-id/[PROJECT_CODE]-app tagged priority:high"
```
```
"Show me all pull requests waiting for review in qtrust-id/[PROJECT_CODE]-app"
```
```
"What labels exist in the qtrust-id/[PROJECT_CODE]-app repository?"
```

If Claude can answer these, your GitHub MCP is working correctly.

---

## 8. Usage Examples by Role

### Project Manager

```
"Create 5 GitHub Issues for the [Module Name] sprint in qtrust-id/[PROJECT_CODE]-app.
Each issue should include: a clear title, a description, acceptance criteria,
labels (module:[name], type:feature, team:[name]), and a story point estimate."
```

```
"What issues are currently in the 'In Progress' column of the [PROJECT_CODE] project board?"
```

### Backend / Frontend Engineer

```
"Show me all issues assigned to me in qtrust-id/[PROJECT_CODE]-app with status In Progress."
```

```
"What is the status of the CI/CD run for the latest commit on branch feature/[branch-name]?"
```

```
"Summarise the review comments on PR #[number] in [PROJECT_CODE]-app."
```

### QA / QC

```
"Create a bug report issue for this problem: [describe the bug].
Add labels: type:bug, priority:high, module:[module-name], team:qa."
```

```
"List all open issues labelled type:bug in [PROJECT_CODE]-app sorted by date created."
```

### CTO / IT Head

```
"Give me a summary of all open PRs across qtrust-id repos that have been open for more than 3 days."
```

```
"How many issues were closed this sprint in [PROJECT_CODE]-app?"
```

---

## 9. Branching Convention

All engineers must follow this branching convention:

| Branch | Purpose |
|---|---|
| `main` | Production-ready code only. Never commit directly. |
| `develop` | Active development integration branch. |
| `feature/[issue-number]-[short-description]` | New features. Branch from `develop`. |
| `fix/[issue-number]-[short-description]` | Bug fixes. Branch from `develop`. |
| `hotfix/[issue-number]-[short-description]` | Critical production fixes. Branch from `main`. |

```bash
# Example: starting work on issue #42 — login module
git checkout develop
git pull origin develop
git checkout -b feature/42-login-module
```

---

## 10. First-Day Checklist

- [ ] GitHub account created (or updated) with `@qtrust.id` email
- [ ] Added to the `qtrust-id` organisation with the correct role
- [ ] Organisation invitation accepted — `qtrust-id` visible on github.com
- [ ] SSH key generated and added to GitHub (or PAT configured)
- [ ] Project repository cloned to local machine
- [ ] GitHub MCP running locally (Docker + PAT via `claude_desktop_config.json`) — or `gh` CLI configured as fallback
- [ ] Verification test passed — can list issues via Claude

---

*Last updated: 2026-06-04 — QTrust IT*
