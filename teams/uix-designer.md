# Onboarding Guide — UIX Designer

**Team:** UIX Designer  
**Role in HRIS:** Translate product requirements into intuitive, visually consistent, and accessible user experiences across web and mobile platforms.

---

## 1. Welcome

You are responsible for the look, feel, and usability of the entire HRIS application. Your designs are the direct interface between the system and its users — HR admins, managers, and employees who will use this product every day. Good design reduces training time, prevents errors, and makes people's working lives easier.

On this project, you work primarily in **Figma** and collaborate closely with Product Development (for requirements) and Frontend/Mobile Engineers (for implementation). Claude Desktop with the Figma MCP integration will significantly accelerate your workflow.

---

## 2. Your Responsibilities

**Research & Strategy**
- Conduct or review user research to understand how HR staff and employees currently work
- Define user flows and information architecture before creating any visual designs
- Document design decisions and their rationale in `by-team/uix/design-decisions.md`

**Visual Design**
- Build and maintain the HRIS Design System (colors, typography, spacing, components)
- Create low-fidelity wireframes for all screens before moving to high-fidelity
- Produce high-fidelity mockups for all modules: web and mobile
- Ensure designs are responsive (desktop-first web, native patterns for mobile)

**Handoff & Collaboration**
- Export assets and annotate designs for Frontend and Mobile Engineers
- Write handoff notes for every completed module in `by-team/uix/handoff-notes/`
- Participate in design reviews with Product, Frontend, and Mobile teams
- Respond to implementation questions from engineers promptly

**Quality**
- Conduct design QA — compare implemented UI against your Figma mockups
- Ensure all designs meet WCAG 2.1 AA accessibility standards
- Keep Figma files organized, named consistently, and free of unused components

---

## 3. Tools & Access Setup

### 3.1 Figma
- You need a **Figma Editor** seat. Request this from your team lead.
- Sign in with your `@qtrust.id` Google Workspace account
- You will be added to the QTrust Figma workspace
- Create your working files under the `HRIS` project in the shared workspace
- Update `by-team/uix/figma-links.md` whenever you create a new Figma file

### 3.2 Claude Desktop + Figma MCP
- Install Claude Desktop and sign in with your QTrust premium account
- The Figma MCP is already integrated — you can push and read Figma designs directly from Claude
- Connect your Google Drive workspace folder in Claude Desktop settings
- **How to use Claude with Figma:**
  - *"Create a new Figma file for the HRIS design system"*
  - *"Add a wireframe for the attendance check-in screen to the HRIS wireframes file"*
  - *"Read the current design from [Figma URL] and generate the Blade component"*

### 3.3 Google Drive
- Request access to the shared HRIS Google Drive folder
- Your primary output folders:
  - `HRIS/designs/wireframes/` — low-fidelity exports
  - `HRIS/designs/mockups/` — high-fidelity exports (PNG 2x, PDF)
  - `HRIS/designs/assets/` — icons, images, logos
  - `HRIS/designs/design-system/` — design system documentation
  - `HRIS/by-team/uix/` — working notes and design decisions

### 3.4 GitHub
- Request read access to `github.com/qtrust/hris-app`
- You will primarily read issues tagged `team:uix` and leave design feedback comments
- Optionally: use GitHub Issues to track design tasks in parallel with the PM's workflow

---

## 4. Design System

The design system is the single most important foundation you will build. Everything else — wireframes, mockups, mobile screens — must use design system components. Build it before creating any feature screens.

### Figma File Structure
```
[File] HRIS — Design System
  ├── Page: Cover
  ├── Page: Foundations
  │   ├── Color Palette (tokens)
  │   ├── Typography Scale
  │   ├── Spacing & Grid
  │   ├── Shadows & Borders
  │   └── Icons
  ├── Page: Components
  │   ├── Buttons
  │   ├── Form Elements (Input, Select, Checkbox, Radio, Datepicker)
  │   ├── Navigation (Sidebar, Breadcrumb, Tabs)
  │   ├── Data Display (Table, Badge, Card, List)
  │   ├── Feedback (Toast, Modal, Alert, Empty State)
  │   └── Layout (Page Shell, Section, Grid)
  └── Page: Patterns
      ├── Forms (Create, Edit)
      ├── Tables with Filters
      └── Approval Flows

[File] HRIS — Wireframes
  ├── Page: Auth (Login, Forgot Password)
  ├── Page: Dashboard
  ├── Page: Employee Management
  ├── Page: Attendance
  ├── Page: Leave Management
  ├── Page: Payroll
  ├── Page: Performance
  ├── Page: Recruitment
  └── Page: Settings

[File] HRIS — Hi-Fi Mockups (Web)
  └── [Mirror of wireframe pages, high fidelity]

[File] HRIS — Mobile (iOS + Android)
  ├── Page: Android — [screen name]
  └── Page: iOS — [screen name]
```

### Design Tokens (pre-defined in `designs/design-system/README.md`)

| Token | Value | Usage |
|---|---|---|
| `color-primary` | #1E40AF | Primary actions, active states |
| `color-secondary` | #0F766E | Secondary actions |
| `color-success` | #16A34A | Approved, active, positive |
| `color-warning` | #D97706 | Pending, caution |
| `color-danger` | #DC2626 | Error, rejected, destructive |
| `color-bg` | #F3F4F6 | Page background |
| `color-surface` | #FFFFFF | Card, modal background |
| `font-sans` | Inter | All text |
| `radius-md` | 8px | Cards, modals |
| `radius-sm` | 4px | Buttons, inputs |

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
| Files | `HRIS — [Category]` | `HRIS — Hi-Fi Mockups` |
| Pages | `[Module] — [Status]` | `Attendance — WIP` |
| Frames | `[Screen]/[State]` | `CheckIn/Default`, `CheckIn/Loading` |
| Components | `PascalCase` | `AttendanceBadge`, `LeaveCard` |
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

Mobile screens live in the `HRIS — Mobile` Figma file. Follow platform conventions strictly.

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
> "Create a wireframe in Figma for the Leave Management module of an HRIS app. Include screens for: leave balance overview, submit leave request form, leave history list, and manager approval inbox. Use a clean, corporate style."

**Generate a design system component:**
> "Add a Status Badge component to the HRIS design system Figma file. It should have variants for: Approved (green), Pending (yellow), Rejected (red), and Cancelled (grey)."

**Read Figma designs for handoff:**
> "Read the Figma frame at [URL] and describe the layout, components, and spacing in detail so I can write handoff notes."

**Generate icon SVGs:**
> "Create SVG icons for: clock-in, clock-out, leave calendar, payslip document, and org chart. Use a 24×24 outline style consistent with Heroicons."

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
| Design System Tokens | `designs/design-system/README.md` |
| Mobile UX Notes | `by-team/mobile/ux-notes.md` |
| All PRDs (requirements) | `docs/product/` |
| Handoff Notes | `by-team/uix/handoff-notes/` |
| Figma Design Best Practices | [figma.com/best-practices](https://www.figma.com/best-practices/) |
| Material Design 3 | [m3.material.io](https://m3.material.io) |
| Apple HIG | [developer.apple.com/design/human-interface-guidelines](https://developer.apple.com/design/human-interface-guidelines) |
| WCAG 2.1 | [w3.org/TR/WCAG21](https://www.w3.org/TR/WCAG21/) |
