---
id: KB-CCC1A552
subject: BL-SR-017 Persisted block order and `hidden` are independent, verbatim round-trip fields
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:29.866Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-017: Persisted block order and `hidden` are independent, verbatim round-trip fields `[P2-ux]`

- **Rule:** Within a `SalesRepLayoutRegion.blocks[]`, array position is the only signal of render order and `hidden` is a flag independent of position — both are stored and returned exactly as sent, with no server-side reordering, deduplication, or reinterpretation.
- **Verify:** Save a region with blocks in a specific order and one block `hidden:true`; reload → same order, same hidden flags (SR-GQL-100/108/109/115).
- **Violation signal:** Reload returns blocks in a different order than saved; a `hidden:true` block reverts to `false` (or vice versa) without a save.
- **Agents:** qa-backend-expert
- **Source:** `vc-module-sales-rep` `LayoutBlock`/`LayoutRegion` (plain properties, no reordering logic); `LayoutService.SaveLayoutAsync`/`GetLayoutAsync` (verbatim JSON serialize/deserialize, no transform).
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04; source + live CONFIRM; docs N/A).
