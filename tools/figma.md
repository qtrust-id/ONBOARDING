# Tool Setup Guide — Figma MCP

> **Required for:** UIX Designer (Editor), All other roles (Viewer)
> **Role:** Design tool for all wireframes, mockups, and the design system. MCP integration lets Claude generate frames and read design specs directly from conversations.

---

## 1. What It Is

Figma is QTrust's visual design tool. The Figma MCP integration in Claude Desktop enables two powerful workflows:

**Design → Code:** Claude reads a Figma frame and generates the corresponding UI component (Blade, React, Flutter, etc.).

**Prompt → Design:** Claude creates wireframes, components, and layouts directly in Figma from a text description.

This makes UIX Designers significantly faster and allows Frontend/Mobile Engineers to generate code that precisely matches the designs.

---

## 2. Access Levels by Role

| Role | Figma Access | Seat Type |
|---|---|---|
| UIX Designer | Create and edit all files | **Editor** (paid seat — request from IT Head) |
| CTO / IT Head | Full access | **Admin** |
| Product Development | View and comment | **Viewer** (free) |
| Project Manager | View and comment | **Viewer** (free) |
| Frontend Engineer | View, inspect, comment | **Viewer** (free) |
| Mobile Apps Engineer | View, inspect, comment | **Viewer** (free) |
| QA / QC | View for visual QA | **Viewer** (free) |
| Backend Engineer | Not required | — |
| Image Vision Engineer | Not required | — |
| DevOps | Not required | — |

> **UIX Designers only:** You need an **Editor** seat, which is a paid Figma seat. Ask the IT Head to assign one to you before you start. Without it, you can view but cannot create or edit designs.

---

## 3. Account Setup

1. Go to [figma.com](https://figma.com) and click **Sign up**
2. Use your `@qtrust.id` Google account to sign in (select "Continue with Google")
3. Ask the IT Head to add you to the QTrust Figma organisation with the correct role
4. Accept the invitation email from Figma
5. You should now see the QTrust organisation in your Figma workspace

---

## 4. Project Files You Will Find

When added to the QTrust organisation, you will see a project named `[PROJECT_NAME]` containing:

| File | Contents | Who Edits |
|---|---|---|
| `[PROJECT_NAME] — Design System` | Color tokens, typography, components | UIX Designer |
| `[PROJECT_NAME] — Wireframes` | Low-fidelity user flows and screen layouts | UIX Designer |
| `[PROJECT_NAME] — Hi-Fi Mockups` | High-fidelity screens with all states | UIX Designer |
| `[PROJECT_NAME] — Mobile` | iOS and Android-specific screens | UIX Designer |

File links are documented in `by-team/uix/figma-links.md` in the project Google Drive folder.

---

## 5. Connecting Figma MCP in Claude Desktop

1. Open Claude Desktop → **Settings** → **Integrations**
2. Find **Figma** and click **Connect**
3. You will be redirected to Figma to authorise the connection
4. Log in with your `@qtrust.id` Google account and grant access
5. Return to Claude Desktop — the connection should show **Connected**

> **Note:** The Figma MCP is already pre-configured in the QTrust Cowork setup. If you see Figma listed as active in your project's Claude conversation, it is already connected at the project level. You may still need to connect your personal account for full access.

---

## 6. Verification Test

Run these prompts in Claude Desktop to confirm the integration is working:

```
"List my Figma files."
"Read the design from [Figma file URL] and describe the layout of the first frame."
```

---

## 7. Usage Examples by Role

**UIX Designer — Generate wireframe:**

```
"Create a wireframe in Figma in the [PROJECT_NAME] — Wireframes file.
Add a new page called '[Module] — WIP'.
Create a frame for the [screen name] screen. Include: [describe key UI elements].
Use a clean, professional style consistent with the existing design system."
```

**UIX Designer — Add component to design system:**

```
"Add a Status Badge component to the [PROJECT_NAME] — Design System file.
Variants: Approved (green), Pending (amber), Rejected (red), Cancelled (grey).
Follow the existing naming convention in the file."
```

**Frontend/Mobile Engineer — Read design for implementation:**

```
"Read the Figma frame at [URL].
Describe the exact layout, spacing values, typography, and colours I need to implement this screen.
Use the design token names from the design system where possible."
```

**QA / QC — Visual QA reference:**

```
"Read the Figma frame at [URL] for the [screen name].
List all the visual states that should be tested: default, loading, empty, error, disabled."
```

**CTO — Design progress check:**

```
"List all pages in the [PROJECT_NAME] — Wireframes Figma file.
Which ones are still marked WIP?"
```

---

## 8. Dev Mode (For Engineers)

Engineers should use Figma's **Dev Mode** when implementing designs:

1. Open the Figma file
2. Click the **</>** icon (Dev Mode) in the top right toolbar
3. Click any element to see exact dimensions, spacing, colour values, and CSS/iOS/Android code snippets
4. Use this alongside Claude's Figma MCP output for accurate implementation

---

## 9. First-Day Checklist

- [ ] Figma account created with `@qtrust.id` Google SSO
- [ ] Added to QTrust Figma organisation with correct role (Editor or Viewer)
- [ ] Can access all project design files
- [ ] Figma MCP connected in Claude Desktop (run verification test)
- [ ] UIX Designers: Editor seat confirmed — can create and edit frames
- [ ] Engineers: Dev Mode accessible in design files
