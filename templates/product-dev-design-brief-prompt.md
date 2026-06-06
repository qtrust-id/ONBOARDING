# Sample Prompt — Product Development Design Brief for UIX

> **Audience:** Product Development
> **Purpose:** A ready-to-use sample prompt that turns an **approved PRD** into a designer-ready **design brief** for the UIX Designer, inside a Claude Cowork project. It bridges the Product Development handoff (Workflow step 5 in [`../teams/product-development.md`](../teams/product-development.md)) to the start of the UIX workflow (steps 1–5 in [`../teams/uix-designer.md`](../teams/uix-designer.md)).
> **Version:** 1.0 — June 2026

---

## 1. What This Sample Does

A PRD answers *what* and *why*. A design brief distills an **approved** PRD into *which screens, which flows, which states, which data* — the form a designer can act on without re-reading the whole PRD. Run once per module in a configured Cowork project, this prompt has Claude:

- Read the approved PRD from the workspace and the UIX Designer guide (read from GitHub via the browser) so the brief matches how the designer actually works.
- Produce a design brief in a fixed 12-section format, saved under `docs/product/design-briefs/`.
- List every screen per platform with all mandatory states, derive everything from the PRD (no invented features), and route gaps to a consolidated Open Questions list.

The brief does **not** replace the PRD — it links to it. A PRD must be **Approved** before a brief is written; the prompt checks this and stops if the PRD is still Draft.

The example below is written for the **QTrust HRIS** project. Adapt the module name, file paths, and platform mix to your own project — the structure stays the same.

---

## 2. Prerequisites

- A Cowork project (e.g. `QT-HRIS`) with Project Instructions filled in (see [`product-dev-prd-prompt.md`](product-dev-prd-prompt.md) §4 or [`cowork-instructions.md`](cowork-instructions.md)).
- The project's **workspace folder connected**, containing the **approved** PRD at `docs/product/PRD-[module].md`.
- **Claude in Chrome** active (used to read the public ONBOARDING repo).
- The ONBOARDING repo public at `https://github.com/qtrust-id/ONBOARDING`.

---

## 3. The Design Brief Format

The prompt produces these 12 sections, mapped to the UIX workflow:

1. **Header** — Module; link to the source PRD (must be Approved); Author; Date; Target platforms (Web / Mobile); personas/roles affected.
2. **Design Goal & Context** — 2–3 sentences on what the user must accomplish and what success looks like.
3. **Personas & Devices** — which roles use this, on what device, in what context.
4. **Screen Inventory** — a table of every screen, split by platform, each with a one-line purpose.
5. **Primary User Flows** — numbered step-by-step flows, including any approval flow.
6. **Per-Screen Requirements** — for each screen: data/fields shown, key actions, mandatory states (Default, Loading, Empty, Error, Disabled, No-permission), validation messages, edge cases.
7. **Design System & Constraints** — which existing tokens/components to reuse; flag any new component.
8. **Accessibility & Platform** — WCAG 2.1 AA notes, touch-target sizes, mobile frame sizes, Material 3 / iOS HIG conventions.
9. **Content & Copy Notes** — labels, empty-state text, error-message tone.
10. **Out of Scope (design)** — what should not be designed this round.
11. **Open Questions for Product** — design-blocking questions.
12. **Definition of Done (design)** — wireframe coverage for all screens and states, reviewed and approved.

---

## 4. The Sample Prompt

Copy the block below into Cowork and run it once per module (replace `[module]`).

```text
You are a member of the Product Development team at QTrust working on the QTrust HRIS project. An approved PRD now needs to be turned into a design brief for the UIX Designer. Claude drafts; I review and approve.

Before you start, READ these references:
1. The approved PRD from the workspace: docs/product/PRD-[module].md (e.g. PRD-employee.md). Confirm its header Status is "Approved" — if it still says "Draft", stop and tell me, because UIX should only design against approved PRDs.
2. teams/uix-designer.md from the public qtrust-id/ONBOARDING repo, read THROUGH THE BROWSER (Claude in Chrome) at:
   https://github.com/qtrust-id/ONBOARDING/blob/main/teams/uix-designer.md?plain=1
   Use it so the brief matches how the designer works — Section 4 (Design System tokens), Section 6 (mandatory screen states, Figma naming, accessibility checklist) and Section 7 (mobile frame sizes / platform conventions). If a URL cannot be opened, stop and tell me — do not guess.

Create a task list to track progress. Write the brief in concise, professional English Markdown and save it to docs/product/design-briefs/[module]-design-brief.md (create the folder if needed).

Produce the brief using EXACTLY these sections:
1. Header — Module; link to the source PRD (note it must be Approved); Author = Product Development; Date = today; Target platforms (Web / Mobile); Personas/roles affected.
2. Design Goal & Context — 2-3 sentences on what the user must accomplish and what a successful design looks like.
3. Personas & Devices — which roles use this, on what device, in what context.
4. Screen Inventory — a table of every screen, split by platform (Web vs Mobile), each with a one-line purpose.
5. Primary User Flows — numbered step-by-step flows, including any approval flow (e.g. request -> manager -> HR).
6. Per-Screen Requirements — for each screen: data/fields shown, key actions, the mandatory states to design (Default, Loading, Empty, Error, Disabled, No-permission), validation messages, and edge cases.
7. Design System & Constraints — which existing design-system tokens/components to reuse (reference the tokens from the UIX guide); flag any new component that must be created.
8. Accessibility & Platform — WCAG 2.1 AA notes, touch-target sizes, mobile frame sizes, Material 3 / iOS HIG conventions where mobile applies.
9. Content & Copy Notes — labels, empty-state text, and the tone for error messages.
10. Out of Scope (design) — what should NOT be designed in this round.
11. Open Questions for Product — any design-blocking questions for me to answer.
12. Definition of Done (design) — wireframe coverage for all listed screens and states, reviewed and approved by Product + PM.

Derive every requirement from the PRD — do not invent features. Where the PRD is silent on something a designer needs (e.g. a field, a state, an empty-message), add it under Open Questions instead of assuming.

When finished: list the file path, the number of screens per platform, and the consolidated Open Questions. Verify the file was saved by reading it back, and confirm every screen in the Screen Inventory has its states defined in Section 6.
```

---

## 5. After the Brief — Handing Off to UIX

1. **Answer the Open Questions** the prompt surfaced, and fold the answers back into the brief.
2. **Share the brief + the approved PRD link** with the UIX Designer (Figma/Drive comment + Slack), and point them to the Figma file structure (Design System / Wireframes / Hi-Fi / Mobile).
3. The designer starts at UIX Workflow step 1 (read PRD/brief) → step 2 (user-flow) → step 3 (wireframes), raising any further questions as Figma/Drive comments.
4. In parallel, the PM converts user stories into GitHub Issues and links each to its Milestone.

---

*Maintained by QTrust Engineering · https://github.com/qtrust-id/ONBOARDING*
