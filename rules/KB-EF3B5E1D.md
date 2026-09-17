---
id: KB-EF3B5E1D
subject: BL-CART-004 Currency switching recalculates primary-currency lines
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:00.958Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CART-004: Currency switching recalculates primary-currency lines `[P0-revenue]`

- **Rule:** When the user switches the cart currency, every line item **denominated in the previous cart currency** must recalculate using the new currency's price list. If such a product has no price in the new currency, that line item must be flagged or removed. Shipping and tax recalculate. **Mixed Cart exception (Loyalty mode "Mixed Cart"):** line items denominated in a non-primary currency (e.g. a PTS loyalty line) are NOT converted — they are preserved at their original currency, so a cart MAY legitimately hold multiple currencies simultaneously. See BL-LOY-006 for the switch contract and BL-LOY-003 for the per-currency totals split.
- **Verify:** Single-currency cart: add items → switch currency → all prices, subtotal, shipping, tax use the new currency; products without a new-currency price are flagged/removed. Mixed cart: with a PTS loyalty line present, switch the base currency USD→EUR → USD lines convert to EUR, the PTS line stays PTS, item count unchanged.
- **Violation signal:** A previous-currency line still uses the old price; subtotal/shipping not recalculated; **OR** (Mixed Cart) a non-primary-currency loyalty line is wrongly converted, dropped, or duplicated on currency switch.
- **Agents:** qa-frontend-expert (cart UI), qa-backend-expert (xAPI cart recalculation)
- **Amended:** 2026-06-09 (Mixed Cart — VCST-5101). Previously asserted "no mixed-currency state"; superseded by the Mixed Cart model — see Domain 17 (BL-LOY).
