# Monthly Business Review (MBR) — [Month YYYY]

> **Audience:** CEO & top-level management
> **Prepared by:** [CTO / BizOps]
> **Period covered:** [YYYY-MM-01] to [YYYY-MM-DD]
> **Confidentiality:** Confidential
> **Distribution:** [CEO, COO, CFO, … ]

> **How to use this template:** This is the monthly strategic-layer output described in [`../executive-business-layer.md`](../executive-business-layer.md). Fill every `[placeholder]` with a sourced figure. Keep it business-framed — outcomes, not operational metrics. Claude can draft the whole brief from the connected tools (see the prompt at the end); a human reviews and signs off before distribution.

---

## 1. Executive Summary

> Three to five sentences a CEO can read in 60 seconds. Lead with overall health, then the headline outcomes and the single most important decision needed.

**Overall health:** 🟢 On track / 🟡 At risk / 🔴 Off track

- [Headline outcome 1 — e.g., "Verification module shipped 6 days ahead of target; partner adoption +18% MoM."]
- [Headline outcome 2 — e.g., "Infrastructure cost per verification down 9%; gross margin improving."]
- [Headline outcome 3 — e.g., "99.95% uptime across all services; one open compliance item before the ISO audit."]
- **Decision needed:** [the one thing leadership must decide this month — or "None"]

---

## 2. Business Outcomes by Dimension

> One table per dimension. For each KPI: this month, last month, target, trend (▲ ▼ ▬), and the source tool so any number is traceable.

### 2.1 Time-to-Value

| KPI | This month | Last month | Target | Trend | Source |
|---|---|---|---|---|---|
| Lead time, idea → production (median) | [x days] | [x days] | [x days] | [▲/▼/▬] | Linear + GitHub |
| Roadmap commitments delivered on time | [x%] | [x%] | [≥ x%] | | Linear |
| Releases shipped to production | [n] | [n] | [n] | | GitHub |

### 2.2 Product Impact

| KPI | This month | Last month | Target | Trend | Source |
|---|---|---|---|---|---|
| Active verifying partners | [n] | [n] | [n] | | PostHog / CRM |
| Products registered | [n] | [n] | [n] | | Product analytics |
| Verifications / scans performed | [n] | [n] | [n] | | Product analytics |
| Warranty claims processed | [n] | [n] | [n] | | Product analytics |
| New-feature adoption | [x%] | [x%] | [≥ x%] | | PostHog |
| Counterfeit catch / verification success rate | [x%] | [x%] | [≥ x%] | | Product analytics |

### 2.3 Efficiency & ROI

| KPI | This month | Last month | Target | Trend | Source |
|---|---|---|---|---|---|
| Cloud spend (total) | [Rp x] | [Rp x] | [≤ Rp x] | | Datadog + billing export |
| Cost per verification (unit economics) | [Rp x] | [Rp x] | [≤ Rp x] | | Billing ÷ analytics |
| Engineering capacity on strategic work | [x%] | [x%] | [≥ x%] | | Linear |

### 2.4 Risk & Trust

| KPI | This month | Last month | Target | Trend | Source |
|---|---|---|---|---|---|
| Uptime / SLA attainment | [x%] | [x%] | [≥ 99.9%] | | Datadog |
| Production incidents (count / max severity) | [n / sev] | [n / sev] | [0 critical] | | Sentry + Datadog |
| Mean time to resolve | [x h] | [x h] | [≤ x h] | | Sentry/Datadog |
| Open security / compliance findings | [n] | [n] | [0 high] | | Security review |

### 2.5 Strategic Alignment

| KPI | This month | Last month | Target | Trend | Source |
|---|---|---|---|---|---|
| Effort on strategic initiatives vs maintenance | [x% / y%] | [x% / y%] | [target split] | | Linear |
| Company OKR progress (see §6) | [x%] | [x%] | [on plan] | | OKR doc |

---

## 3. Portfolio Health

> Every active project scored on Value, Risk, and Cost. Use 🟢 / 🟡 / 🔴. This is the leadership view for resource-allocation decisions (continue / accelerate / pause).

| Project | Stage | Value | Risk | Cost | Status | Note |
|---|---|---|---|---|---|---|
| [PROJECT_CODE] | [Discovery/Build/Live] | 🟢/🟡/🔴 | 🟢/🟡/🔴 | 🟢/🟡/🔴 | On track / At risk | [one line] |
| [PROJECT_CODE] | | | | | | |
| [PROJECT_CODE] | | | | | | |

---

## 4. Financials & ROI

> The money view. Pull from the finance connector (Xero / QuickBooks) and CRM.

| Metric | This month | Last month | Target / Budget | Source |
|---|---|---|---|---|
| Revenue (total / per product line) | [Rp x] | [Rp x] | [Rp x] | Finance / CRM |
| Gross margin | [x%] | [x%] | [≥ x%] | Finance |
| Top customers / partners by revenue | [list] | — | — | Finance / CRM |
| New partners onboarded | [n] | [n] | [n] | CRM |
| Partner retention / churn | [x% / y%] | [x% / y%] | [≥ x% / ≤ y%] | CRM |
| Cloud cost as % of revenue | [x%] | [x%] | [≤ x%] | Billing ÷ Finance |

**ROI narrative:** [1–2 sentences tying the spend to the return — e.g., "Each Rp1 of infra spend supported Rp X of verified-product revenue this month."]

---

## 5. Risks & Compliance

| Risk / Issue | Impact | Likelihood | Owner | Status / Mitigation |
|---|---|---|---|---|
| [Risk 1] | High/Med/Low | High/Med/Low | [name] | [action] |
| [Risk 2] | | | | |

- **Incidents this month:** [summary — what happened, customer impact, resolution]
- **Compliance:** [audits due, regulatory items — e.g., UU PDP / ISO 27001 status]

---

## 6. Strategic Alignment & OKR Progress

| Company Objective | Key Result | Progress | On plan? |
|---|---|---|---|
| [Objective 1] | [KR — measurable] | [x%] | 🟢/🟡/🔴 |
| [Objective 2] | [KR] | [x%] | |

**Commentary:** [Where engineering effort advanced the objectives; where it diverged and why.]

---

## 7. Decisions Needed from Leadership

> Make it easy to say yes/no. State the decision, the options, the recommendation, and the deadline.

| # | Decision | Options | Recommendation | Needed by |
|---|---|---|---|---|
| 1 | [decision] | [A / B] | [rec] | [date] |
| 2 | | | | |

---

## 8. Appendix — Data Sources & Methodology

| Dimension | Source tool(s) | Notes / caveats |
|---|---|---|
| Time-to-Value | Linear, GitHub | [definition of "lead time" used] |
| Product Impact | PostHog / Amplitude, CRM | [what counts as "active"] |
| Efficiency & ROI | Datadog, billing export, Xero/QuickBooks | [cost-allocation method] |
| Risk & Trust | Sentry, Datadog, security review | [SLA window] |
| Strategic Alignment | Linear, OKR doc | [effort estimation basis] |

*Every figure above is traceable to its source. Where a number is estimated rather than measured, it is flagged inline.*

---

## How to Generate This Brief with Claude

Once the executive connectors are active (see [`../executive-business-layer.md`](../executive-business-layer.md) §4), prompt Claude:

```
"Generate the Monthly Business Review for [Month YYYY] using the MBR template.
Pull live data from the connected tools and fill every KPI table:
- Linear + GitHub  → Time-to-Value (lead time, roadmap adherence, releases)
- PostHog + CRM    → Product Impact (active partners, registrations, verifications, adoption)
- Datadog + billing → Efficiency & ROI (cloud cost, cost per verification)
- Sentry + Datadog → Risk & Trust (uptime, incidents, MTTR)
- Xero/QuickBooks  → Financials (revenue, margin, top partners)
- OKR doc          → Strategic Alignment
For every figure, cite the source tool. Flag any metric you could not retrieve
as 'data unavailable' rather than estimating. Keep the Executive Summary to 5 sentences."
```

Then review every number against its source before distribution. **AI assists; the producer signs off.**

> **Automate it:** schedule this prompt to run on the first business day of each month, producing a draft MBR for review. See the scheduling guidance in the onboarding tools.

---

*Template maintained by QTrust Engineering · https://github.com/qtrust-id/ONBOARDING*
