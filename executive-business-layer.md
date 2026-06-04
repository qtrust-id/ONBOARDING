# Executive Business Layer

> **Audience:** CEO and top-level management (with the CTO as the producer/owner)
> **Purpose:** Turn the engineering workflow's signals into business outcomes leadership can act on.
> **Version:** 1.0 — June 2026

---

## 1. Philosophy

The QTrust engineering workflow already produces rich operational data — sprint progress in Linear, releases in GitHub, errors in Sentry, infrastructure metrics in Datadog, decisions in Fireflies. That data answers the CTO's question: *"Are our projects healthy and on track?"*

It does **not**, by itself, answer the CEO's question: *"Is our investment in product turning into business value?"*

This document defines the **translation layer** that sits on top of the engineering workflow and converts technical signals into business outcomes.

> **Core principle:** Leadership does not consume operational metrics. Every number that reaches the CEO is framed as a business outcome — value delivered, adoption, ROI, risk, or strategic progress — with its source traceable back to the operational layer.

### Three Layers of Visibility

| Layer | Owner | Core question | Cadence | Primary surfaces |
|---|---|---|---|---|
| Operational | Team & Tech Lead | What is being worked on; what is blocked? | Daily | Linear, GitHub, Slack standups |
| Tactical | CTO | Are projects on-track, healthy, and secure? | Weekly | CTO morning brief (see `tools/README.md`) |
| **Strategic / Business** | **CEO / Top Management** | **Is product investment producing business value?** | **Monthly / Quarterly** | **MBR brief, CEO Cockpit, Portfolio Health** |

The most common failure is pushing an operational dashboard at the CEO. The strategic layer must answer business questions, not display raw technical metrics.

---

## 2. The Five Business Dimensions

Every executive output is organised around these five dimensions. Example KPIs are framed for QTrust's business (digital trust, anti-counterfeiting, product registration, warranty) — adapt the specific metrics per product line.

### 2.1 Time-to-Value
*How fast does an idea become a product customers can use?*

- Lead time: idea → production (median, per project)
- Roadmap commitments delivered on time (%)
- Releases shipped to production per month

### 2.2 Product Impact
*Is what we shipped actually used, and is it moving the business?*

- Partner / end-user adoption of new features (%)
- Core volume metrics: products registered, verifications/scans performed, warranty claims processed
- North-star metric per product (e.g., monthly active verifying partners)
- Counterfeit catch rate / verification success rate

### 2.3 Efficiency & ROI
*What does each project cost, and what does it return?*

- Cloud spend per project and per environment
- Unit economics (e.g., infrastructure cost per verification)
- Engineering effort allocation (% of capacity per project)
- Cost-to-value ratio per initiative

### 2.4 Risk & Trust
*Is the business exposed?* — existential for a company that sells trust.

- Uptime / SLA attainment per service
- Security & compliance posture (open findings, audits due — e.g., UU PDP / ISO 27001)
- Production incidents: count, severity, customer impact, time-to-resolve
- Trust index: counterfeit/fraud events caught vs missed

### 2.5 Strategic Alignment
*Is engineering effort aligned to company objectives?*

- % effort on strategic initiatives vs maintenance / keeping-the-lights-on
- Progress against company OKRs
- Portfolio balance: new bets vs core platform vs technical debt

---

## 3. The Translation Layer

The operational data already exists. The executive layer rolls it up and re-frames it:

| Operational signal (already captured) | Business KPI it feeds |
|---|---|
| Linear cycles + GitHub releases | Time-to-Value: lead time, roadmap adherence, release cadence |
| Sentry + Datadog (uptime, errors, latency) | Risk & Trust: SLA attainment, incident impact, trust index |
| Datadog cost + cloud billing export | Efficiency & ROI: cost per project, unit economics |
| Linear effort/estimates | Strategic Alignment: effort allocation across initiatives |
| Fireflies (exec/stakeholder meetings) | Decisions and commitments, traceable |
| **Product analytics** *(new — §4)* | Product Impact: adoption, active partners, conversion |
| **CRM** *(new — §4)* | Product Impact & ROI: revenue, partner pipeline, retention/churn |
| **Finance/accounting** *(new — §4)* | Efficiency & ROI: revenue, P&L, top customers |

The gap the new connectors close: QTrust is strong on *delivery & reliability* signals, but lacks *adoption & revenue* signals needed to complete the sentence "this feature produced X for the business."

---

## 4. The Executive Tool Stack

These connectors extend the existing stack (Linear, GitHub, Sentry, Datadog, Fireflies, Google Calendar) with the business-side data the strategic layer needs. All are available as MCP connectors and connect per-user in Claude Desktop. Connect deliberately and in phases — start with the recommended set.

| Need | Recommended | Alternatives | What it unlocks |
|---|---|---|---|
| Product analytics | **PostHog** | Amplitude, Mixpanel, Pendo | Feature adoption, active partners, funnels (registration, verification, warranty claim) |
| CRM / revenue | **HubSpot** | Zoho CRM, Day AI | Revenue, partner pipeline, retention/churn linkage |
| Finance / P&L | **Xero** or **QuickBooks** | Zoho Books, Rillet | P&L, cash position, top customers by revenue, benchmarking |
| Partner program *(QTrust-specific)* | **EULER** | — | Partner Data Lake, referrals, partner deals |
| Cloud cost | **Datadog** (already in stack) + billing export → Google Sheets | — | Cost per project / per environment for ROI |
| BI / unified data *(optional, later)* | **Metabase** or **ThoughtSpot** | Polar Analytics | One analytical layer across all sources when scale demands it |

> **No dedicated GCP/AWS cloud-billing MCP exists in the registry today.** Until one does, derive cost-per-project from Datadog cost metrics plus a monthly billing export into a Google Sheet that Claude reads.

### The Executive Intelligence Loop

```
Engineering connectors (Linear · GitHub · Sentry · Datadog)
        +
Business connectors    (PostHog · HubSpot · Xero · EULER)
        ▼
Claude aggregates and re-frames into business outcomes
        ▼
MBR brief · CEO Cockpit · Portfolio Health · OKR report
```

This complements the existing Calendar → Fireflies → Claude loop: that one captures *what was discussed*; this one captures *what the business achieved*.

---

## 5. Executive Deliverables & Cadence

| Deliverable | Format | Cadence | Produced by |
|---|---|---|---|
| **Monthly Business Review (MBR) brief** | Document (see [`templates/monthly-business-review-template.md`](templates/monthly-business-review-template.md)) | Monthly | CTO / BizOps via Claude, can be scheduled |
| **CEO Cockpit** | Live one-page view (persistent artifact) pulling fresh KPIs | On-demand, always current | CTO / BizOps |
| **Portfolio Health view** | Projects scored on Value × Risk × Cost (RAG) | Monthly, inside the MBR | CTO |
| **Quarterly OKR Alignment report** | Document mapping effort and results to company objectives | Quarterly | CTO + CEO |

Respect the cadence pyramid: the CEO gets the monthly business view and quarterly strategy — not the daily operational firehose. The MBR is the anchor; the cockpit is for the CEO who wants to self-serve between briefs.

---

## 6. Roles & Ownership

- **Producer:** the CTO (or a BizOps / Chief-of-Staff function) owns generating the executive outputs using Claude across the connectors.
- **Automation:** the MBR brief can be generated by a **scheduled task** at the start of each month, then reviewed before distribution.
- **Consumer:** CEO and top management consume the MBR and cockpit; they raise decisions back through the "Decisions Needed" section.
- **Source of truth:** operational tools remain the source of truth; the executive layer never invents numbers — it aggregates and re-frames.

---

## 7. Governance

- **AI assists, humans decide.** Every figure in an executive output must be sourced and verifiable; the producer reviews and signs off before distribution. "Claude generated it" is never the explanation for a wrong number in front of the board.
- **Least-privilege access.** Finance, CRM, and revenue connectors carry sensitive data — grant access only to those who need it, and classify the outputs accordingly (Confidential / Restricted).
- **Single re-framing, not duplication.** The executive layer points back to the operational tools; it does not create a parallel set of conflicting metrics.

---

## 8. Getting Started (Phased Rollout)

| Phase | Action | Outcome |
|---|---|---|
| 1 | Connect the starter set: PostHog (or Amplitude), HubSpot (or Zoho), Xero (or QuickBooks); optionally EULER | Adoption + revenue + P&L data flowing into Claude |
| 2 | Produce the first **MBR brief** from the template, manually, for one month | A working business-framed report; refine the KPIs |
| 3 | Schedule the MBR generation; stand up the **CEO Cockpit** artifact | Recurring, self-serve executive visibility |
| 4 | Add Portfolio Health scoring and the Quarterly OKR report | Full strategic layer operational |

> Do not connect everything at once. Each connector added must earn its place by feeding a KPI the CEO actually asks about.

---

*Maintained by QTrust Engineering · https://github.com/qtrust-id/ONBOARDING*
