# PRD — [Module/Feature Name]

> **Status:** `Draft` | `In Review` | `Approved` | `Deprecated`  
> **Author:** [Name]  
> **Reviewers:** [Names]  
> **Last Updated:** YYYY-MM-DD  
> **GitHub Milestone:** [link]  
> **Project:** [PROJECT_NAME]

---

## 1. Background & Problem Statement

> Describe the current situation and the pain point this feature addresses. Explain why it matters to the business and to users. Include any data or context that justifies building this now (e.g., support ticket volume, user feedback, regulatory requirement, competitive pressure). Keep this to 2–3 paragraphs.

[Describe the current pain point and the business justification for solving it here.]

---

## 2. Goals

> List measurable success outcomes. Each goal should be verifiable — avoid vague statements like "improve performance." Tie goals to metrics wherever possible.

**This feature will:**

- [ ] [Measurable outcome 1 — e.g., Reduce manual data entry time by 50% for warehouse operators]
- [ ] [Measurable outcome 2 — e.g., Achieve product authentication scan accuracy ≥ 95%]
- [ ] [Measurable outcome 3 — e.g., Allow users to complete the core task in ≤ 3 taps on mobile]

**This feature will NOT:**

- [Out-of-scope item 1 — e.g., Replace the existing ERP system integration]
- [Out-of-scope item 2 — e.g., Support offline mode in this release]

---

## 3. User Personas

| Persona | Description | Primary Need |
|---|---|---|
| [Persona Name, e.g., Warehouse Operator] | [Brief description — role, context, technical comfort level] | [What they need this feature to do for them] |
| [Persona Name, e.g., QA Supervisor] | [Brief description] | [Primary need] |
| [Persona Name, e.g., System Administrator] | [Brief description] | [Primary need] |

---

## 4. User Stories

> Use the format: **"As a [role], I want to [action] so that [outcome]."**  
> Each story must have acceptance criteria written in Given / When / Then format.

### Story 1 — [Short title]

**As a** [role], **I want to** [action] **so that** [outcome].

**Acceptance Criteria:**

- **Given** [initial context or state]  
  **When** [user action or system event]  
  **Then** [expected observable outcome]

- **Given** [initial context or state]  
  **When** [user action or system event]  
  **Then** [expected observable outcome]

---

### Story 2 — [Short title]

**As a** [role], **I want to** [action] **so that** [outcome].

**Acceptance Criteria:**

- **Given** [initial context or state]  
  **When** [user action or system event]  
  **Then** [expected observable outcome]

---

### Story 3 — [Short title]

**As a** [role], **I want to** [action] **so that** [outcome].

**Acceptance Criteria:**

- **Given** [initial context or state]  
  **When** [user action or system event]  
  **Then** [expected observable outcome]

---

## 5. Functional Requirements

> Number requirements with the R-NNN format. Mark priority: **P0** (must-have for launch), **P1** (important but not blocking), **P2** (nice-to-have).

| ID | Priority | Requirement |
|---|---|---|
| R-001 | P0 | [Describe the required behavior precisely. Avoid "the system should" — use "the system must."] |
| R-002 | P0 | [Functional requirement 2] |
| R-003 | P1 | [Functional requirement 3] |
| R-004 | P1 | [Functional requirement 4] |
| R-005 | P2 | [Functional requirement 5] |

---

## 6. Non-Functional Requirements

| Requirement | Target | Notes |
|---|---|---|
| **Performance** | [e.g., API response time p95 ≤ 500ms under 100 concurrent users] | [Any context or trade-offs] |
| **AI / Inference Latency** | [e.g., /predict endpoint p95 ≤ 200ms — if applicable] | [N/A if no AI component] |
| **Security** | [e.g., All endpoints require Bearer token; no sensitive data in response logs] | [Specific risks to address] |
| **Accessibility** | [e.g., Web UI must meet WCAG 2.1 AA; minimum touch target 44×44pt on mobile] | [Platform-specific notes] |
| **Availability** | [e.g., 99.5% uptime SLA; max 30 min planned downtime per deploy] | [Affects deployment strategy] |
| **Data Retention** | [e.g., Audit logs retained for 2 years; user uploads retained for 1 year] | [Regulatory requirement?] |
| **Scalability** | [e.g., Must support up to 500 concurrent users without degradation] | [Peak load scenario] |

---

## 7. API Contract (Draft)

> This is a high-level draft for early alignment between Product, Backend, and Mobile/Frontend. The authoritative spec lives in `docs/api/openapi.yaml`.

| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| `POST` | `/api/v1/[resource]` | [Create a new resource] | Yes |
| `GET` | `/api/v1/[resource]/{id}` | [Retrieve a single resource] | Yes |
| `GET` | `/api/v1/[resource]` | [List resources with pagination] | Yes |
| `PUT` | `/api/v1/[resource]/{id}` | [Update a resource] | Yes |
| `DELETE` | `/api/v1/[resource]/{id}` | [Soft-delete a resource] | Yes |

**Key request/response notes:**

- [Note 1 — e.g., All timestamps returned in ISO 8601 UTC format]
- [Note 2 — e.g., Paginated lists use `data`, `meta.current_page`, `meta.total` envelope]
- [Note 3 — e.g., Errors follow the standard QTrust error envelope: `success`, `data`, `message`, `errors`]

---

## 8. UI/UX Notes

> Key interaction patterns and constraints for the UIX Designer. This is not a full design spec — it is the functional intent that design must satisfy.

**Key interaction patterns:**

- [Pattern 1 — e.g., The scan action must be reachable in ≤ 2 taps from the home screen]
- [Pattern 2 — e.g., Results must be displayed inline — no modal dialogs for the primary success state]
- [Pattern 3 — e.g., Error states must show a user-friendly message and a clear retry action]

**Constraints:**

- [Constraint 1 — e.g., Must work on low-end Android devices (2GB RAM)]
- [Constraint 2 — e.g., Form inputs must support autofill]
- [Constraint 3 — e.g., Loading states must be shown for any action taking > 300ms]

**Figma reference:** [Link to relevant Figma screens, if any already exist]

---

## 9. Out of Scope

The following items are explicitly **excluded** from this release:

- [Excluded item 1 — e.g., Bulk import via CSV — tracked in GitHub Issue #NNN]
- [Excluded item 2 — e.g., Offline mode]
- [Excluded item 3 — e.g., Admin-side reporting dashboard]
- [Excluded item 4 — e.g., Integration with third-party ERP system]

> If a stakeholder requests one of these items during the sprint, refer them back to this document and create a new issue for prioritization.

---

## 10. Open Questions

| Question | Owner | Deadline | Status |
|---|---|---|---|
| [Question 1 — e.g., What is the maximum image size accepted by the AI serving endpoint?] | [Vision Engineer] | YYYY-MM-DD | Open |
| [Question 2 — e.g., Does this feature require a separate user permission level, or is it role-based?] | [Product / IT Head] | YYYY-MM-DD | Open |
| [Question 3 — e.g., Are there regulatory constraints on storing scanned product images?] | [Business Owner / Legal] | YYYY-MM-DD | Open |

---

## 11. Dependencies

| Dependency | Type | Owner | Risk |
|---|---|---|---|
| [Dependency 1 — e.g., AI model /predict endpoint must be deployed before mobile integration] | Internal | [Vision Engineer] | High — blocks mobile sprint |
| [Dependency 2 — e.g., Figma designs for all screens must be approved before frontend starts] | Internal | [UIX Designer] | Medium |
| [Dependency 3 — e.g., Third-party barcode lookup API — rate limits and pricing TBD] | External | [IT Head] | Medium |
| [Dependency 4 — e.g., GCP Cloud Run quota increase request submitted] | External | [DevOps] | Low |

---

## 12. Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | YYYY-MM-DD | [Name] | Initial draft |
| 1.1 | YYYY-MM-DD | [Name] | [Description of changes] |
| 1.2 | YYYY-MM-DD | [Name] | [Description of changes] |
