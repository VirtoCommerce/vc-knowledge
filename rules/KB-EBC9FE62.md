---
id: KB-EBC9FE62
subject: BL-ORD-010 Order totals — one entry per distinct line currency, unique default-currency flag
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: CustomerOrderType.orderTotals
  - coordinate: CustomerOrderType.currency
evidence:
  - method: observation
    at: 2026-09-17T15:21:04.478Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-ORD-010: Order totals — one entry per distinct line currency, unique default-currency flag `[P1-data]`

- **Rule:** `CustomerOrder.OrderTotals[]` MUST contain exactly one entry per distinct currency present across `Items[].Currency`, `Shipments[].Currency`, and `InPayments[].Currency`. The entry whose `CurrencyCode == CustomerOrder.Currency` MUST have `isDefaultTotalCurrency = true`; no other entry may. Each entry's `total/subTotal/taxTotal/discountTotal` reflect only that currency's entities. A single-currency order → exactly 1 entry. The REST `WithOrderTotals` response group (bit 9 = 512) must be requested; without it `OrderTotals` is null (not empty).
- **Verify:** Single-currency USD order → `order { orderTotals { isDefaultTotalCurrency total{currency{code}} } }` → exactly 1 entry, USD, `isDefaultTotalCurrency=true`. Mixed (USD + PTS) order → exactly 2 entries, exactly one `isDefaultTotalCurrency=true`, each subTotal = sum of its-currency lines. REST GET without `WithOrderTotals` → `orderTotals` null.
- **Violation signal:** Duplicate currency entries; zero or two `isDefaultTotalCurrency=true`; an entry's subtotal mixes currencies; `OrderTotals` populated without the flag requested.
- **Agents:** qa-backend-expert, qa-frontend-expert
- **Source:** vc-module-order #497 `DefaultCustomerOrderTotalsCalculator.CalculateTotals` + `OrderTotal.cs`; vc-module-x-order #43 `CustomerOrderAggregate.OrderTotals` / `CustomerOrderType.cs:179`; live-verified 2026-06-22 on the environment. Covered by suites 075b/083b.
- **Promoted:** 2026-06-23.

---
