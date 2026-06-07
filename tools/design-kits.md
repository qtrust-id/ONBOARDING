# Tool Setup Guide — Design Kits & the QTrust Design System

> **Required for:** UIX Designer (Editor), Frontend & Mobile Engineers (consumers).
> **Status:** Canonical design-system model for all QTrust projects.
> **Version:** 2.0 — June 2026
>
> QTrust does **not** build a bespoke component library. We standardise on **three standard kits** plus a thin **QTrust brand layer**. Components come from the kits; we only maintain brand tokens layered on top.

---

## 1. The model in one picture

| Platform | Primary kit | Format |
|---|---|---|
| **Web** | **Preline UI** | **Figma library + Tailwind code** (both free/open-source) |
| **Android** | **Material 3 Design Kit** | Figma community library |
| **iOS** | **iOS & iPadOS 26 UI Kit** | Figma community library |
| **All** | **QTrust Brand Layer** | Figma variables + code theme |

**Web is Preline UI** — a free, open-source Tailwind CSS library that ships **both** a Figma kit **and** matching Tailwind code. This gives design↔code alignment at no cost, and unifies the workflow: every platform is now **Figma-first by instancing a kit**, then implemented against that kit's code/specs.

> **Tailwind Plus = optional secondary source.** QTrust holds a Tailwind Plus licence; its blocks may be used as an **extra** source of web patterns (Application UI), but it is **not** the standard and has **no Figma kit** (code-only). Prefer Preline for anything that needs a Figma source. See §6.

---

## 2. Kits, sources & licensing

| Kit | Platform | Source | Cost / License |
|---|---|---|---|
| **Preline UI** (Figma) | Web | https://www.figma.com/community/file/1179068859697769656/preline-ui-figma | **Free** — personal, commercial & client use ([preline.co/figma](https://preline.co/figma/)). |
| **Preline UI** (code) | Web | https://preline.co · https://github.com/htmlstreamofficial/preline | **Free / open-source** (MIT). Tailwind CSS + headless JS plugins; framework-agnostic — works with Blade/Livewire. |
| **Material 3 Design Kit** | Android | https://www.figma.com/community/file/1035203688168086460/material-3-design-kit | **Free** (Figma community file). |
| **iOS & iPadOS 26 UI Kit** | iOS | https://www.figma.com/community/file/1527721578857867021/ios-and-ipados-26 | **Free** (Figma community file). |
| **Tailwind Plus** _(optional)_ | Web | https://tailwindcss.com/plus/ui-blocks/application-ui | Teams licence (already owned). Code-only, **no Figma**. Optional extra block source — not the standard. May not be redistributed as a derivative UI kit. |

All four required kits are **free**. Preline replaces the earlier "web = Tailwind Plus, design-in-code" approach.

---

## 3. One-time setup (IT Head / CTO — manual)

1. **Duplicate the kits into the QTrust Figma org** (not personal drafts) and **publish each as a Team Library**:
   - Preline UI Figma → rename `QTrust — Preline (Web)`
   - Material 3 Design Kit → `QTrust — Material 3 (Android)`
   - iOS & iPadOS 26 UI Kit → `QTrust — iOS 26 (iOS)`
   Publishing is **mandatory** — it lets project files *instance* components/variables and avoids the Figma-MCP "freshly created page/component doesn't persist" issue.
2. **Create & publish `QTrust — Brand Layer`** (Figma file): a `qtrust/brand` variable collection (colors incl. the AA-safe status-text variants, radius) + Inter type styles, layered on top of the kits. Preline ships Figma variables/30 palettes — point its primary palette at the QTrust brand.
3. **Install Preline in the web codebase** (`hris-app`): `npm i preline`, add Preline to the Tailwind config/`@plugin`, and set the QTrust tokens in the Tailwind theme (`@theme` / CSS variables) so code matches the Brand Layer.
4. **Enable the libraries** in each project's design files (Wireframes + per-platform), so nothing is rebuilt per project.

---

## 4. The QTrust Brand Layer (the only thing we maintain)

Carry over the accessibility-audited tokens (the success/warning **text** variants exist because the fills fail WCAG AA as text — see `teams/uix-designer.md` §6).

| Token | Value | Web (Preline / Tailwind theme) | Android (Material 3 role) | iOS (HIG) |
|---|---|---|---|---|
| `brand-primary` | `#1E40AF` | Preline primary palette / `--color-primary` | `Primary` (dynamic color off) | Accent / tint |
| `brand-secondary` | `#0F766E` | `--color-secondary` | `Secondary` | Secondary accent |
| `success` (fill) | `#16A34A` | `--color-success` | success role | system green / custom |
| `success-text` | **`#15803D`** | success **text** only | success text | success label |
| `warning` (fill) | `#D97706` | `--color-warning` | warning | custom |
| `warning-text` | **`#B45309`** | warning **text** only | warning text | warning label |
| `danger` | `#DC2626` | `--color-danger` | `Error` | system red / custom |
| `neutral-grey` | (Cancelled status) | `--color-neutral` | neutral/disabled | secondaryLabel |
| `bg` / `surface` | `#F3F4F6` / `#FFFFFF` | `--color-bg` / `--color-surface` | surface roles | systemBackground |
| Type | Inter / Roboto / SF Pro | Inter | Roboto (system) | SF Pro (system) |
| Radius | `sm 4` · `md 8` | `--radius-sm/-md` | M3 shape tokens | corner radius |

**Golden rule:** apply the brand **into** each kit's tokens — never fight platform conventions. iOS keeps **SF Pro + system controls**; Android keeps **Material 3 + Roboto**; web uses **Preline** components recolored to the QTrust palette. Status is **never color alone** (text + icon); status **text** uses the AA-safe `*-text` variants.

---

## 5. How each role uses the kits

**UIX Designer (Figma-first on every platform)**
- **Web:** enable `QTrust — Preline (Web)` + `QTrust — Brand Layer`; build wireframes and hi-fi web screens by **instancing Preline components**; apply the Brand Layer for color/accent.
- **Android / iOS:** instance `QTrust — Material 3` / `QTrust — iOS 26` + Brand Layer.
- Never redraw or rebuild kit components; module-specific components are **wrappers/extensions** over kit primitives, documented in `by-team/uix/design-decisions.md`.

**Frontend Engineer (web)**
- Implement with **Preline UI** (Tailwind + Preline JS plugins) as **Blade components**, swapping Preline's plain JS for **Livewire + Alpine** where reactivity is needed; theme with the QTrust Tailwind tokens. Designs map 1:1 to Preline components, so handoff is direct.

**Mobile Engineer**
- Implement against the kit specs: Flutter **Material 3 `ThemeData`** (Android) and the **iOS 26 kit + HIG** (iOS), themed with the Brand Layer. Use Figma **Dev Mode** for measurements.

---

## 6. Working with Claude / Figma MCP

- **Prefer instancing published-library components** over MCP-generating from scratch — more reliable and avoids the non-persistence issue. **Build on existing pages.**
- **Web design→code is now direct:** because Preline ships matching code, Claude can turn a Preline-based Figma screen (or a Preline component name) into Blade/Livewire markup with QTrust tokens.
- **Tailwind Plus (optional):** if a needed pattern only exists in Tailwind Plus, use the render harness in `TailwindPlus/_figma-harness/` to preview/import it — but treat it as a one-off; Preline is the standard.
- **Quota:** kits are large. Read **specific nodes**, avoid screenshots, reuse. Heavy users → **Premium** seat ([`README.md` ▸ Managing Token Usage & Quota](README.md#managing-token-usage--quota)).

---

## 7. Governance

- The **Brand Layer is the single source of truth** for QTrust identity (Figma variables + code theme kept in sync). Version it.
- **Do not fork kit components.** If one genuinely needs changing, **wrap/extend** it as a small QTrust component and record why in `by-team/uix/design-decisions.md`.
- **Accessibility stays QTrust-owned.** Kits don't guarantee your contrast choices: enforce WCAG 2.1 AA (use the `*-text` variants), visible focus, labelled inputs, touch targets, and status-not-by-color-only.
- **Keep Preline code + Figma in lockstep:** pin the Preline version in `hris-app` and note the matching Figma kit version here. Update this doc on any kit major version (Preline, Material, iOS, or Tailwind CSS).

---

*Maintained by QTrust Engineering · [`qtrust-id/ONBOARDING`](https://github.com/qtrust-id/ONBOARDING)*
