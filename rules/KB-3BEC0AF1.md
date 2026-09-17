---
id: KB-3BEC0AF1
subject: BL-LOY-003 Mixed Cart — `cartTotals` exposes one entry per distinct line currency
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: CartType.cartTotals
  - coordinate: CartType.currency
evidence:
  - method: observation
    at: 2026-09-17T15:21:20.938Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-003: Mixed Cart — `cartTotals` exposes one entry per distinct line currency `[P1-data]`

- **Rule:** `cart.cartTotals[]` MUST contain exactly one entry per distinct currency present among the cart's line items. The entry with `isDefaultTotalCurrency = true` MUST correspond to `cart.currency`. Each entry's `subTotal`/`discountTotal`/`taxTotal`/`total` reflect ONLY that currency's lines. A primary-currency-only cart → exactly one entry.
- **Verify:** Mixed cart (USD + PTS) → query `cartTotals { isDefaultTotalCurrency total { currency { code } } subTotal { amount } }` → assert exactly 2 entries; the default entry's currency = cart currency; each `subTotal` = that currency's lines' sum.
- **Violation signal:** Single entry on a mixed cart (grouping broken); zero or multiple `isDefaultTotalCurrency = true` entries; a block's totals include other-currency values.
- **Agents:** qa-backend-expert (xAPI), qa-frontend-expert (split-by-currency summary)
- **Source:** vc-module-x-cart PR #120 `CartType.cartTotals` + vc-module-cart PR #188 `DefaultShoppingCartTotalsCalculator` (`cartsByCurrency`). `CartTotalType` = `{ isDefaultTotalCurrency, total, subTotal, taxTotal, discountTotal }`. Suite: `050b4` GQL-MC-002, `CRX-GQL-096`.
- **Promoted:** 2026-06-09.
