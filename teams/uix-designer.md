# Onboarding Guide — UIX Designer

**Team:** UIX Designer  
**Role Overview:** Translate product requirements into intuitive, visually consistent, and accessible user experiences across web and mobile platforms.

---

## 1. Welcome

You are responsible for the look, feel, and usability of the entire application. Your designs are the direct interface between the system and its users — who will use this product every day. Good design reduces training time, prevents errors, and makes people's working lives easier.

On this project, you work primarily in **Figma** and collaborate closely with Product Development (for requirements) and Frontend/Mobile Engineers (for implementation). Claude Desktop with the Figma MCP integration will significantly accelerate your workflow.

---

## 2. Your Responsibilities

**Research & Strategy**
- Conduct or review user research to understand how users currently work
- Define user flows and information architecture before creating any visual designs
- Document design decisions and their rationale in `by-team/uix/design-decisions.md`

**Visual Design**
- Adopt and maintain the **QTrust Brand Layer** on top of the three standard kits (**Preline UI** for web / Material 3 / iOS 26) — see [`../tools/design-kits.md`](../tools/design-kits.md). Do **not** build a bespoke component library.
- Create low-fidelity wireframes for all screens before moving to high-fidelity
- Produce high-fidelity mockups on **every** platform by **instancing the Figma kits** (web = Preline UI; Android = Material 3; iOS = iOS 26)
- Ensure designs are responsive (desktop-first web, native patterns for mobile)

**Handoff & Collaboration**
- Export assets and annotate designs for Frontend and Mobile Engineers
- Write handoff notes for every completed module in `by-team/uix/handoff-notes/`
- Participate in design reviews with Product, Frontend, and Mobile teams
- Respond to implementation questions from engineers promptly

**Quality**
- Conduct design QA on the **staging** build — compare the implemented UI against your Figma mockups before it is promoted to production (3-tier flow: [`../environments-and-promotion.md`](../environments-and-promotion.md))
- Ensure all designs meet WCAG 2.1 AA accessibility standards
- Keep Figma files organized, named consistently, and free of unused components

---

## 3. Tools & Access Setup

### 3.1 Figma
- You need a **Figma Editor** seat. Request this from your team lead.
- Sign in with your `@qtrust.id` Google Workspace account
- You will be added to the QTrust Figma workspace
- Create your working files under the `[PROJECT_NAME]` project in the shared workspace
- Update `by-team/uix/figma-links.md` whenever you create a new Figma file

### 3.2 Claude Desktop + Figma MCP
- Install Claude Desktop and sign in with your QTrust premium account
- The Figma MCP is already integrated — you can push and read Figma designs directly from Claude
- Connect your Google Drive workspace folder in Claude Desktop settings
- **How to use Claude with Figma:**
  - *"Create a new Figma file for the [PROJECT_NAME] design system"*
  - *"Add a wireframe for the [feature] screen to the [PROJECT_NAME] wireframes file"*
  - *"Read the current design from [Figma URL] and generate the UI component"*

### 3.3 Google Drive
- Request access to the shared project Google Drive folder
- Your primary output folders:
  - `[PROJECT_CODE]/designs/wireframes/` — low-fidelity exports
  - `[PROJECT_CODE]/designs/mockups/` — high-fidelity exports (PNG 2x, PDF)
  - `[PROJECT_CODE]/designs/assets/` — icons, images, logos
  - `[PROJECT_CODE]/designs/design-system/` — design system documentation
  - `[PROJECT_CODE]/by-team/uix/` — working notes and design decisions

### 3.4 GitHub
- Request read access to the repository (URL in Project Configuration Sheet)
- You will primarily read issues tagged `team:uix` and leave design feedback comments
- Optionally: use GitHub Issues to track design tasks in parallel with the PM's workflow

---

## 4. Design System — three standard kits + a QTrust Brand Layer

**We do not build a bespoke component library.** QTrust standardises on three industry-standard kits and maintains only a thin **brand layer** on top. Full setup, licensing, and governance live in **[`../tools/design-kits.md`](../tools/design-kits.md)** — read it before you start. Summary:

| Platform | Kit (components come from here) | Format |
|---|---|---|
| **Web** | **Preline UI** — free Tailwind CSS kit | **Figma library + Tailwind code** (both free) |
| **Android** | **Material 3 Design Kit** | Figma community library |
| **iOS** | **iOS & iPadOS 26 UI Kit** | Figma community library |
| **All** | **QTrust Brand Layer** (color/type/status/radius) | Figma variables + code theme |

> **Figma-first on every platform.** Web now has a real Figma kit (Preline UI), so you design web in Figma like mobile — no more "design-in-code only". Preline also ships matching Tailwind code, so Frontend implements it 1:1 in Blade/Livewire. _Tailwind Plus is an optional secondary block source only — see `tools/design-kits.md` §6._

### How you work with the kits
- Enable the published `QTrust — Preline (Web)`, `QTrust — Material 3`, `QTrust — iOS 26`, and `QTrust — Brand Layer` libraries in the project's design files, then build screens by **instancing kit components** — do not redraw them, do not rebuild the kit.
- Apply the Brand Layer for color/accent only, and never fight platform conventions (iOS keeps SF Pro + system controls; Android keeps Material 3 + Roboto; web uses Preline recolored to the QTrust palette).

### Figma File Structure
```
[File] QTrust — Preline (Web)          ← duplicated kit, published library (org-level)
[File] QTrust — Material 3 (Android)   ← duplicated kit, published library (org-level)
[File] QTrust — iOS 26 (iOS)           ← duplicated kit, published library (org-level)
[File] QTrust — Brand Layer            ← brand tokens, published library (org-level)

[File] [PROJECT_NAME] — Wireframes     ← low-fi for web + mobile (one page per module)
[File] [PROJECT_NAME] — Web            ← hi-fi web, instancing Preline UI
[File] [PROJECT_NAME] — Mobile         ← hi-fi mobile (Android + iOS), instancing the kits
  ├── Page: Android — [module]
  └── Page: iOS — [module]
```

### QTrust Brand Tokens (the only thing we maintain — full table in `tools/design-kits.md` §4)

| Token | Value | Note |
|---|---|---|
| `brand-primary` | #1E40AF | Primary actions / accent / M3 Primary / iOS tint |
| `brand-secondary` | #0F766E | Secondary |
| `success` / `success-text` | #16A34A / **#15803D** | Fill vs **AA-safe text** (fill fails AA as text) |
| `warning` / `warning-text` | #D97706 / **#B45309** | Fill vs **AA-safe text** |
| `danger` | #DC2626 | Error / rejected / destructive |
| `neutral-grey` | — | Cancelled / inactive status |
| `bg` / `surface` | #F3F4F6 / #FFFFFF | Page / card background |
| Type | Inter / Roboto / SF Pro | Web / Android / iOS |
| `radius-sm` / `radius-md` | 4px / 8px | Inputs/buttons / cards/modals |

---

## 5. Workflow

```
1. Read the PRD
   ↓ Understand requirements, personas, and constraints
   ↓ Clarify ambiguities with Product Development before drawing anything

2. User Flow Diagram
   ↓ Map out all screens and transitions for the module
   ↓ Review with Product and PM — agree on scope before proceeding

3. Low-Fidelity Wireframes
   ↓ Use Claude + Figma MCP to generate initial frames
   ↓ Refine manually in Figma
   ↓ Export to designs/wireframes/ for documentation

4. Design Review (Internal)
   ↓ Share Figma link with Product Development and PM
   ↓ Collect feedback via Figma comments
   ↓ Revise until wireframes are approved

5. High-Fidelity Mockups
   ↓ Apply design system components
   ↓ Include all states: default, hover, loading, empty, error
   ↓ Create both light and dark variants if required

6. Design Review (Engineering)
   ↓ Share with Frontend and Mobile Engineers
   ↓ Discuss implementation feasibility
   ↓ Adjust designs based on technical constraints

7. Asset Export & Handoff
   ↓ Export PNG 2x to designs/mockups/
   ↓ Export SVG icons to designs/assets/icons/
   ↓ Write handoff notes in by-team/uix/handoff-notes/[module].md
   ↓ Annotate spacing, component names, and interaction notes in Figma

8. Design QA
   ↓ Review implemented screens against Figma mockups
   ↓ File bugs in GitHub Issues with label type:bug, team:uix
```

---

## 6. Design Standards & Conventions

### Figma Naming Conventions
| Element | Convention | Example |
|---|---|---|
| Files | `[PROJECT_NAME] — [Category]` | `[PROJECT_NAME] — Mobile` |
| Pages | `[Module] — [Status]` | `[MODULE_NAME] — WIP` |
| Frames | `[Screen]/[State]` | `CheckIn/Default`, `CheckIn/Loading` |
| Components | `PascalCase` | `StatusBadge`, `RequestCard` |
| Layers | `kebab-case` | `header-nav`, `form-input-email` |
| Variants | Property=Value | `State=Hover`, `Size=Large` |

### Screen States — Always Design All Of These
- **Default** — the standard loaded state
- **Loading / Skeleton** — data is being fetched
- **Empty** — no records exist yet
- **Error** — something went wrong
- **Disabled** — action is not available
- **Hover / Focus** — interactive feedback

### Accessibility Checklist
- [ ] Color contrast ratio ≥ 4.5:1 for normal text, ≥ 3:1 for large text
- [ ] All interactive elements have a visible focus state
- [ ] Icons are accompanied by text labels or `aria-label`
- [ ] Forms have visible labels (not placeholder-only)
- [ ] Touch targets on mobile are at least 44×44pt (iOS) / 48×48dp (Android)

---

## 7. Mobile Design Guidelines

Mobile screens live in the `[PROJECT_NAME] — Mobile` Figma file. Follow platform conventions strictly.

| Aspect | Android (Material 3) | iOS (Human Interface) |
|---|---|---|
| Navigation | Bottom navigation bar | Tab bar at bottom |
| Primary action | Floating Action Button (FAB) | Navigation bar button |
| Typography | Roboto | SF Pro |
| List items | Material ListItem | UITableViewCell style |
| Sheet | Bottom Sheet | Sheet / Half Modal |
| Top bar | TopAppBar | Navigation Bar |

**Frame sizes to design for:**
- Android: 360×800 (baseline), 390×844 (large)
- iOS: 390×844 (iPhone 15 baseline), 430×932 (Pro Max)

---

## 8. Collaboration With Other Teams

| Team | How You Work Together |
|---|---|
| **Product Development** | Receive approved PRDs. Raise design questions as Figma comments or Google Drive comments on the PRD. |
| **Project Manager** | Align on which screens are in scope for the current sprint. Flag if design work is a blocker. |
| **Frontend Engineer** | Share Figma links with inspect mode enabled. Write handoff notes. Answer implementation questions quickly. |
| **Mobile Engineer** | Provide platform-specific Figma frames. Export assets in the correct density (1x, 2x, 3x). |
| **QA / QC** | Share the Figma link so QA can compare implementation against your designs when filing visual bugs. |

---

## 9. Working with Claude Desktop

**Generate initial wireframes via Figma MCP:**
> "Create a wireframe in Figma for the [Module] of [application type]. Include screens for: overview, submission form, history list, and approval inbox. Use a clean, corporate style."

**Build a web screen by instancing Preline UI:**
> "In the [PROJECT_NAME] — Web file, build the [screen] by instancing components from the QTrust — Preline (Web) library and applying the QTrust Brand Layer (primary #1E40AF). Don't redraw components."

**Build a mobile screen by instancing the kits:**
> "In the [PROJECT_NAME] — Mobile file, on the 'Android — [module]' page, build the [screen] by instancing Material 3 components from the QTrust — Material 3 library and applying the QTrust Brand Layer (primary #1E40AF). Don't redraw components."

**Read Figma designs for handoff:**
> "Read the Figma frame at [URL] and describe the layout, components, and spacing in detail so I can write handoff notes."

**Generate icon SVGs:**
> "Create SVG icons for: [list of icons relevant to the project]. Use a 24×24 outline style consistent with Heroicons."

**Recall design review feedback (Fireflies MCP):**
> "Get the Fireflies summary of the design review on [date] and list every piece of feedback and decision about [feature/screen], with who raised it." (Fireflies transcribes design reviews — see [`tools/fireflies.md`](../tools/fireflies.md).)

> **⚡ Usage tip:** Reading Figma via MCP is token-heavy and can exhaust your daily quota fast. Read **specific frames/nodes** (not whole files), **avoid pulling screenshots** when metadata or Dev Mode values are enough, and **reuse** generated output instead of regenerating. Heavy Figma users should request a **Premium** seat. Full guidance: [Managing Token Usage & Quota](../tools/README.md#managing-token-usage--quota).

---

## 10. First Week Checklist

- [ ] Figma editor seat granted and workspace access confirmed
- [ ] Claude Desktop installed, signed in, Figma MCP verified working
- [ ] Google Drive folder synced locally, output folders located
- [ ] All existing PRDs read (`docs/product/`)
- [ ] `by-team/uix/figma-links.md` updated with your Figma files (once created)
- [ ] `designs/design-system/README.md` reviewed — understand the pre-defined tokens
- [ ] Design system Figma file started (this is your first deliverable)
- [ ] Attended design kickoff with Product Development team
- [ ] Introduced yourself to Frontend and Mobile Engineers

---

## 11. Key Resources

| Resource | Location |
|---|---|
| Figma File Links | `by-team/uix/figma-links.md` |
| **Design Kits & Brand Layer** | [`../tools/design-kits.md`](../tools/design-kits.md) — Preline (web) / Material 3 / iOS 26 + QTrust tokens |
| **Preline UI (Figma — web, primary)** | [figma.com/community/file/1179068859697769656](https://www.figma.com/community/file/1179068859697769656/preline-ui-figma) · [preline.co](https://preline.co) |
| Material 3 Design Kit (Figma) | [figma.com/community/file/1035203688168086460](https://www.figma.com/community/file/1035203688168086460/material-3-design-kit) |
| iOS & iPadOS 26 UI Kit (Figma) | [figma.com/community/file/1527721578857867021](https://www.figma.com/community/file/1527721578857867021/ios-and-ipados-26) |
| Tailwind Plus (optional block source) | [tailwindcss.com/plus/ui-blocks/application-ui](https://tailwindcss.com/plus/ui-blocks/application-ui) |
| Mobile UX Notes | `by-team/mobile/ux-notes.md` |
| All PRDs (requirements) | `docs/product/` |
| Handoff Notes | `by-team/uix/handoff-notes/` |
| Figma Design Best Practices | [figma.com/best-practices](https://www.figma.com/best-practices/) |
| Material Design 3 | [m3.material.io](https://m3.material.io) |
| Apple HIG | [developer.apple.com/design/human-interface-guidelines](https://developer.apple.com/design/human-interface-guidelines) |
| WCAG 2.1 | [w3.org/TR/WCAG21](https://www.w3.org/TR/WCAG21/) |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · Figma (editor — requires paid seat) · Google Drive · Slack · GitHub (viewer) · Fireflies

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Figma | [`tools/figma.md`](../tools/figma.md) | Day 1 |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
