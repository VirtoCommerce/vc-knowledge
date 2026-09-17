---
id: KB-9ACD7DDC
subject: BL-LOY-005 Mixed Cart — a loyalty-currency line shows no "earn points" indicator
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: CartType.items
evidence:
  - method: observation
    at: 2026-09-17T15:21:21.327Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-005: Mixed Cart — a loyalty-currency line shows no "earn points" indicator `[P2-ux]`

- **Rule:** When loyalty points are computed for a line item, if `lineItem.Currency == Loyalty.Currency` (the store's configured points currency), the returned `loyaltyPoints` MUST be zero or null. Computing points-on-points from a points-priced item is semantically invalid and MUST NOT be displayed.
- **Verify:** Mode "Mixed Cart", `Loyalty.Currency = "PTS"`; add a PTS line → query `cart.items { loyaltyPoints { amount } }` → assert the PTS line returns null or 0.
- **Violation signal:** A PTS line returns a non-zero `loyaltyPoints` value (suggests earning points on a points purchase).
- **Agents:** qa-frontend-expert, qa-backend-expert
- **Source:** vc-module-loyalty `LineItemTypeHook.CalculatePoints(x.ExtendedPrice, x.ProductId)` has no currency guard; `LoyaltyPointsCalculator` resolves `PointsCurrency` from the `Loyalty.Currency` setting.
- **Promoted:** 2026-06-09.
