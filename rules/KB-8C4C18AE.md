---
id: KB-8C4C18AE
subject: BL-LOY-006 Mixed Cart — currency switch converts primary lines, preserves loyalty lines
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: CartType.currency
evidence:
  - method: observation
    at: 2026-09-17T15:21:21.529Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-006: Mixed Cart — currency switch converts primary lines, preserves loyalty lines `[P1-data]`

- **Rule:** On a cart-currency change in Mixed Cart mode, items whose currency = the PREVIOUS cart currency MUST be converted to the new cart currency; items in any other currency (e.g. PTS loyalty) MUST retain their original currency. No line item is lost or duplicated. This is the Mixed Cart refinement of BL-CART-004.
- **Verify:** Mixed cart USD + PTS, `cart.currency = USD`; `changeCartCurrency → EUR` → USD items now EUR (EUR pricing), PTS items still PTS at original prices, item count unchanged.
- **Violation signal:** PTS items disappear, are duplicated, or are switched to the new base currency on a currency change.
- **Agents:** qa-backend-expert
- **Source:** vc-module-x-cart PR #120 `ChangeCartCurrencyCommandHandler.ResolveTargetCurrency` — `itemCurrencyCode.EqualsIgnoreCase(current.Cart.Currency) ? newCart.Currency : current.GetCurrencyByCode(itemCurrencyCode)`. See BL-CART-004 (amended).
- **Promoted:** 2026-06-09.
