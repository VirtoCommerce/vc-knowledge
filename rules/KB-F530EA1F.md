---
id: KB-F530EA1F
subject: BL-PRICE-002 Tax calculation position
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:20:59.721Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-PRICE-002: Tax calculation position `[P0-revenue]`

- **Rule:** Tax is always calculated AFTER all discounts are applied. Tax base = (line total after discounts), not the pre-discount subtotal. Tax rate depends on the shipping address (destination-based).
- **Verify:** In cart/checkout, compare: `taxTotal` should equal `taxRate × (subtotal - totalDiscount)`, not `taxRate × subtotal`.
- **Violation signal:** Tax amount is higher than expected (calculated on pre-discount price), or tax changes when discount is applied/removed but the math doesn't align.
- **Agents:** qa-frontend-expert (checkout totals), qa-backend-expert (order API)
- **Source:** vc-module-order `OrderTotalsCalculationTest.cs` + vc-module-cart `CartTotalsCalculationTests.cs` — tax computed on the post-discount total.
- **Amended:** 2026-07-22 (triangulated — BL-AUDIT-2026-07-22; CONFIRMED 3/3, Source anchor recorded, Rule unchanged)
