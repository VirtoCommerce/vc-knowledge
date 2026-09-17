---
id: KB-0A1EE3DC
subject: BL-SR-005 Statistics scope excludes flag-cancelled / prototype orders unconditionally
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:27.144Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-005: Statistics scope excludes flag-cancelled / prototype orders unconditionally `[P1-data]`

- **Rule:** The statistics scope (`salesRepCustomerOrderStatistics` / `salesRepCustomerCartStatistics`) excludes orders and carts flagged prototype or cancelled **at the entity level** (`IsPrototype` / `IsCancelled`) — unconditionally, and this does NOT loosen under any named filter. This differs from the order-*list* scope, which deliberately includes cancelled orders so that `Cancelled` is a real list filter (BL-SR-009). An order whose `Status` field merely reads a cancelled-like value **without** the `IsCancelled` flag (e.g. written directly by an external/ERP integration bypassing the platform cancel workflow) is NOT excluded by this scope and correctly counts toward every statistics figure, including the baseline/all-status one.
- **Verify:** A customer with a genuinely cancelled order (`IsCancelled=true`) → every statistics figure excludes it under any filter, including `filter:"Cancelled"`. A customer with a `Status`-only cancelled-looking order (`IsCancelled=false`) → the baseline/all-status figure INCLUDES it.
- **Violation signal:** Flag-cancelled/prototype orders inflate the baseline totals/counts; OR a flag-less, status-only cancelled-looking order is wrongly excluded from the baseline (scope matching on the `Status` string instead of the `IsCancelled`/`IsPrototype` flags).
- **Agents:** qa-backend-expert
- **Docs:** N/A — pre-GA module, no VirtoOZ coverage (§1a).
- **Source:** vc-module-sales-rep `RepOrderScopeQueryExtensions.ApplyRepScope` (default `includeCancelled=false` → filters `!IsPrototype && !IsCancelled`); `CustomerOrderStatisticsService.BuildQuery` calls it with no `includeCancelled` argument, so the exclusion is unconditional regardless of any status filter. Contrast `SalesRepOrderStatusService.BuildQuery`, which passes `includeCancelled: true` for the list scope (BL-SR-009).
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; DRIFT — resolves the conflict between the prior text and the shipped statistics scope. Docs N/A per §1a; Source + Live agree. Live axis caveat: the field shape was observed this run, but the flag-vs-status distinction is corroborated from a captured payload rather than independently reproduced — the environment's fixtures cannot write a `Status`-only cancelled order.)
- **Promoted:** 2026-07-23 (TLC-2026-07-23-1943); restored 2026-07-28.
