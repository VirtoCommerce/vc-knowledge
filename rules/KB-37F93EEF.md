---
id: KB-37F93EEF
subject: BL-SR-001 Statistics periods are inclusive UTC instants, no server truncation; omitted bounds → all-time
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:26.284Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-001: Statistics periods are inclusive UTC instants, no server truncation; omitted bounds → all-time `[P1-data]`

- **Rule:** `salesRepCustomerOrderStatistics` / `salesRepCustomerCartStatistics` accept any number of aliased `period(from, to)` and `comparison(current, previous)` blocks. Both bounds are **inclusive UTC instants** — the caller sends the time component and any local→UTC conversion; the server does **no** date truncation. A `period` with no bounds → all-time (`firstOrderDate` = "customer since"). A per-request loader coalesces identical ranges so a range used by both a period and a comparison is aggregated once.
- **Verify:** Same range requested as an aliased `period` and inside a `comparison` → identical aggregate, one DB pass; a bounded `period` vs a no-bound `period` on the same customer → all-time count ≥ bounded count.
- **Violation signal:** Server re-truncates the caller's bounds to date boundaries; identical ranges aggregated more than once; omitted bounds error instead of all-time.
- **Agents:** qa-backend-expert
- **Source:** module README §Order statistics ("both `period` bounds are inclusive and compared as UTC instants… there is no server-side date truncation"); live probe 2026-07-23.
- **Promoted:** 2026-07-23 (TLC-2026-07-23-1943); restored 2026-07-28.
