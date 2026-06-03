# Tool Setup Guide — Google Drive

> **Required for:** All team members
> **Role:** Shared documentation repository — PRDs, meeting notes, design exports, test plans, sprint reports, and the Project Configuration Sheet all live here.

---

## 1. What It Is

Google Drive (Google Workspace) is QTrust's shared document storage. Every project has a dedicated folder that is synced locally via **Google Drive Desktop** on each team member's machine. Claude Desktop connects to this local folder as its **workspace** — meaning Claude can read and write project files directly.

This is how Claude "knows" your project: it reads the Project Configuration Sheet, PRDs, and onboarding documents from the Google Drive project folder. Without this connection, Claude does not have project context.

---

## 2. Account and Access Setup

1. You should already have a `@qtrust.id` Google Workspace account, provisioned by the IT Head.
   - If you do not, contact the IT Head before proceeding.
2. Sign in to [drive.google.com](https://drive.google.com) with your `@qtrust.id` account to verify it is active.
3. Request access to your project's Google Drive folder — the IT Head will share it directly from Google Drive. You will receive a sharing invitation to your `@qtrust.id` inbox.
4. Accept the invitation and verify you can see the folder in **Shared with me** or **Shared drives**.

---

## 3. Installing Google Drive Desktop

Google Drive Desktop syncs the shared project folder to your local machine, which is required for Claude Desktop to access it.

### macOS

```bash
# Step 1: Download Google Drive Desktop
# Visit: https://www.google.com/drive/download/
# Download and run the installer.

# Step 2: Open Google Drive Desktop from Applications.
# Sign in with your @qtrust.id account.

# Step 3: Choose sync mode when prompted:
#   - "Stream files" — recommended for large projects (files load on demand, save disk space)
#   - "Mirror files" — all files available offline (uses more disk space)
# Select "Stream files" unless you regularly work without internet.

# Step 4: Wait for the initial sync to complete (a checkmark appears on the Drive icon in the menu bar).
```

### Windows

```powershell
# Step 1: Download Google Drive Desktop
# Visit: https://www.google.com/drive/download/
# Download and run the installer.

# Step 2: Sign in with your @qtrust.id account during setup.

# Step 3: The Google Drive folder will appear in File Explorer
# under your user directory (e.g., G:\ or under Quick Access).

# Step 4: Wait for the sync indicator in the system tray to show complete.
```

---

## 4. Finding Your Local Sync Path

After installation, the synced folder path differs by OS and username. You need this exact path to connect Claude Desktop.

### macOS

The path follows this pattern:

```
~/Library/CloudStorage/GoogleDrive-your@qtrust.id/Shared drives/QTrust/[PROJECT_CODE]/
```

Or, if the folder is in My Drive:

```
~/Library/CloudStorage/GoogleDrive-your@qtrust.id/My Drive/[PROJECT_CODE]/
```

**To find the exact path:**
1. Open Finder and navigate to the project folder inside Google Drive.
2. Right-click (or Ctrl+click) the `[PROJECT_CODE]` folder.
3. Select **Get Info**.
4. The full path appears under **Where:**.

### Windows

The path follows this pattern:

```
C:\Users\[username]\Google Drive\Shared drives\QTrust\[PROJECT_CODE]\
```

**To find the exact path:**
1. Open File Explorer and navigate to the project folder.
2. Right-click the `[PROJECT_CODE]` folder → **Properties**.
3. The **Location** field shows the full path.

> **Write down your local path.** You will need it when connecting Claude Desktop in the next step.

---

## 5. Connecting Google Drive to Claude Desktop

1. Open Claude Desktop → **Settings** (gear icon) → **Workspace**.
2. Click **Select Folder** (or the folder icon next to the workspace path).
3. In the file picker, navigate to your local Google Drive sync path from Section 4.
4. Select the `[PROJECT_CODE]` folder (not a parent folder) and confirm.
5. Verify the connection:

```
Ask Claude: "List the files in my workspace."
```

> Expected: Claude returns a list of files including the Project Configuration Sheet.

> **Important:** Each team member connects their own local path. The path looks different on every machine (different username, different OS) but points to the same shared Google Drive folder. You are not overwriting anyone else's setup.

---

## 6. Folder Structure

When you open the project folder, you will find the following structure:

```
[PROJECT_CODE]/
├── [PROJECT_CODE]-project-config.md    ← Read this first — it defines the entire project
├── docs/
│   ├── product/                        ← PRDs, user stories, feature specs
│   ├── api/                            ← OpenAPI/Swagger specs
│   ├── architecture/                   ← System design diagrams and ADRs
│   └── meetings/                       ← Meeting notes (format: YYYY-MM-DD-type-topic.md)
├── designs/                            ← Figma exports, asset files, brand guidelines
├── by-team/                            ← Each team's working folder
│   ├── product/
│   ├── uix/
│   ├── backend/
│   ├── frontend/
│   ├── mobile/
│   ├── imagevision/
│   ├── qa/
│   └── devops/
├── reports/                            ← Sprint reports, QA summary reports
└── src-link/
    └── GITHUB.md                       ← Link to the GitHub repository
```

**Read the Project Configuration Sheet first.** It is the single source of truth for the project: team members, stack, repo URL, folder paths, and integration links.

---

## 7. Collaboration Rules

Following these rules prevents file conflicts and data loss.

| Rule | Reason |
|---|---|
| **Do not store code in Google Drive.** Code lives exclusively in GitHub. | Mixing code and docs in Drive creates version confusion and bypasses code review. |
| **Do not store secrets, credentials, or API keys.** Use GCP Secret Manager. | Google Drive is shared broadly; credentials in Drive are a security risk. |
| **Use Google Docs for files edited by multiple people simultaneously.** | Google Docs handles concurrent edits natively. Markdown files do not — two people editing the same `.md` file at the same time creates a conflict copy. |
| **Use Markdown for structured documents edited by one person at a time.** | PRDs, API specs, and architecture docs are best kept in Markdown for Claude compatibility and version clarity. |
| **Follow the naming convention for meeting notes.** | Format: `YYYY-MM-DD-[type]-[topic].md` — e.g., `2026-06-10-sprint-1-planning.md` |
| **Do not delete files from the shared Drive.** Move them to the `archive/` subfolder instead. | Deletions affect all team members. Archive instead of delete. |

---

## 8. Working with Claude and Google Drive

Once your workspace is connected, Claude can interact with project files directly. Some practical examples:

```
"Read the Project Configuration Sheet and summarise the tech stack."
```

```
"Create a new meeting notes file in docs/meetings/ for today's sprint planning session.
Use the naming convention: 2026-06-10-sprint-1-planning.md"
```

```
"Read the PRD at docs/product/PRD-authentication.md and identify any gaps in the acceptance criteria."
```

```
"What was discussed in the last meeting notes file in docs/meetings/?"
```

---

## 9. First-Day Checklist

- [ ] `@qtrust.id` Google Workspace account active and accessible
- [ ] Signed in to drive.google.com — account is working
- [ ] Project Google Drive folder shared by IT Head — visible in Shared with me or Shared drives
- [ ] Google Drive Desktop installed and signed in with `@qtrust.id` account
- [ ] Initial sync completed — project folder visible on local machine
- [ ] Local sync path noted (you will need it again if you reinstall)
- [ ] Claude Desktop workspace folder connected to the project folder
- [ ] Verification passed — Claude can list files in the workspace
- [ ] Project Configuration Sheet (`[PROJECT_CODE]-project-config.md`) located and read

---

*Last updated: 2026-06-03 — QTrust IT*
