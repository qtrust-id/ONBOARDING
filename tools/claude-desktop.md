# Tool Setup Guide — Claude Desktop

> **Required for:** All team members
> **Role:** AI co-pilot for every workflow — code generation, document drafting, design assistance, data queries, and cross-tool intelligence via MCP integrations.

---

## 1. What It Is

Claude Desktop is the local desktop application that powers QTrust's Claude Cowork workflow. Unlike the web interface, Claude Desktop supports **MCP (Model Context Protocol)** integrations — direct connections to GitHub, Figma, Slack, Linear, Sentry, Datadog, Fireflies, and Google Drive. With these integrations active, Claude can read and write data across your entire toolchain without you switching tabs.

This makes Claude your single interface for actions that would otherwise require opening 5–10 separate tools. You ask in plain language; Claude acts across the stack.

---

## 2. Installation

1. Visit [claude.ai/download](https://claude.ai/download) and download the macOS or Windows installer.
2. Run the installer and launch Claude Desktop.
3. Sign in with your **QTrust-issued Claude premium account credentials** — provided by the IT Head. Do not use a personal Claude account.
4. Verify you are on the correct plan: **Settings → Account → confirm "Claude Pro" or "Claude Team"**.

> **Note:** If you do not yet have account credentials, contact the IT Head before proceeding.

---

## 3. Connecting to Your Project (Cowork)

Claude Desktop uses a **Projects** feature to scope your AI conversations to the correct codebase, documentation, and team context.

1. In the left sidebar, locate the **Projects** section.
2. You will see the project(s) you have been invited to (e.g., `QT-PORTAL`, `QT-SCT`).
3. Click the project name to enter it.
4. The project instructions and workspace folder are pre-configured by the IT Head — you do not need to modify them.
5. All conversations you start inside the project will use that project's context automatically.

> If your project does not appear in the sidebar, you have not yet accepted the invitation. Check your `@qtrust.id` email inbox for the invitation link.

---

## 4. Connecting MCP Integrations

MCP integrations are installed **per-user**. Each team member must connect their own tools. Connecting GitHub on your machine does not connect it for your teammates.

**Steps to connect any MCP integration:**

1. Open Claude Desktop → **Settings** (gear icon, bottom-left).
2. Navigate to **Integrations** or **Connections**.
3. Locate the tool in the list (GitHub, Slack, Figma, Linear, Sentry, Datadog, Fireflies, Google Drive).
4. Click **Connect** and complete the OAuth authentication flow for that tool.
5. Return to a Claude conversation and run a quick test (see Section 5).

For step-by-step instructions per tool, see the corresponding guide in the `tools/` folder:

| Tool | Setup Guide |
|---|---|
| GitHub | `tools/github.md` |
| Google Drive | `tools/google-drive.md` |
| Figma | *(guide coming soon)* |
| Slack | *(guide coming soon)* |
| Linear | *(guide coming soon)* |
| Sentry / Datadog | *(guide coming soon)* |
| Fireflies | *(guide coming soon)* |

---

## 5. Verifying Your Setup

After installation and project access, run the following test prompts in a conversation inside your project:

```
"What project am I working on?"
```
> Expected: Claude describes the project from the pre-configured project instructions.

```
"List the files in my workspace folder."
```
> Expected: Claude lists files from the connected Google Drive project folder.

```
"What tools do you have access to?"
```
> Expected: Claude lists the MCP integrations you have connected.

If any of these fail, refer to Section 8 (Getting Help) below.

---

## 6. Daily Usage Tips

| Tip | Details |
|---|---|
| **Give context, not just commands** | Instead of "write a function", say: "Write a [FRAMEWORK] function that does X, given these constraints: Y. The codebase uses [STACK]." |
| **Reference the Project Config Sheet** | Claude reads the project's config file automatically. It knows your stack, repo URL, and team structure — use this. |
| **Use it across all phases** | Product drafts PRDs, UIX prompts Figma designs, Backend generates migrations, QA writes test cases — Claude is a co-pilot for every role. |
| **Review before committing** | Claude drafts; you decide. Always review AI-generated code and documents before using them in production. |
| **Stay inside the project** | Starting conversations inside your assigned project gives Claude the correct context. Conversations outside the project lose that context. |
| **Be specific about file locations** | When asking Claude to create or edit a file, specify the path: "Create this file at `docs/product/PRD-module-name.md`." |

---

## 7. First-Day Checklist

- [ ] Claude Desktop installed on your machine
- [ ] Signed in with QTrust-issued premium account (not personal)
- [ ] Plan confirmed as Claude Pro or Claude Team in Settings
- [ ] Project invitation accepted — project visible in left sidebar
- [ ] Workspace folder accessible — ask Claude: `"List files in my workspace"`
- [ ] At least one MCP integration connected (start with GitHub or Slack)
- [ ] First conversation started inside the assigned project

---

## 8. Getting Help

**If Claude Desktop is not working:**

| Problem | Action |
|---|---|
| Project not visible in sidebar | Confirm you accepted the invitation from your `@qtrust.id` email |
| Signed in to wrong account | Settings → Sign out → Sign in with QTrust credentials |
| MCP integration shows "Disconnected" | Re-open Settings → Integrations → Reconnect the tool |
| Workspace files not visible | Confirm Google Drive Desktop is installed and synced (see `tools/google-drive.md`) |
| None of the above helps | Restart Claude Desktop and retry; if still failing, contact the IT Head |

---

*Last updated: 2026-06-03 — QTrust IT*
