---
id: KB-9AF70DCE
subject: BL-SR-008 Top sellers ranked by named sort over a period; `take` clamps at 10 (never errors); rows are a line-item snapshot
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:27.794Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-008: Top sellers ranked by named sort over a period; `take` clamps at 10 (never errors); rows are a line-item snapshot `[P1-data]`

- **Rule:** `salesRepTopSellers` ranks products by `sort` (`by-units` default, `by-revenue`) over an optional `period`, returning the top `take` (default 5, **max 10**). `take` above 10 is **clamped** (never a validation error). Each row's `name`/`sku`/`imageUrl`/category come from the **order line-item snapshot** — no live catalog read; `revenue` is Money. Optional category `filter` restricts to that category's subtree. Creator+membership scoped (BL-SR-002); omit `organizationId` for the cross-customer dashboard.
- **Verify:** `take:5` → ≤5 rows; `take:15` → ≤10 rows, no error (live-confirmed 2026-07-23: clamped); `by-units` vs `by-revenue` re-rank; category filter narrows the set; row identity from snapshot even if the catalog product changed.
- **Violation signal:** `take>10` errors or returns >10; ranking reads live catalog; category filter ignored; another rep's line items ranked.
- **Agents:** qa-backend-expert, qa-frontend-expert
- **Source:** module README §Top sellers; live probe 2026-07-23.
- **Promoted:** 2026-07-23 (TLC-2026-07-23-1943); restored 2026-07-28.
