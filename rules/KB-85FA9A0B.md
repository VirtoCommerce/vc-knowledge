---
id: KB-85FA9A0B
subject: BL-SR-006 Cart statistics are currency-scoped; item quantity is the shipped primary metric
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:27.360Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-006: Cart statistics are currency-scoped; item quantity is the shipped primary metric `[P1-data]`

- **Rule:** `salesRepCustomerCartStatistics` uses a cart-*kind* filter whose built-in default `"active-carts"` = non-empty carts that are **not** wishlists. `count` / `total` / `average` remain schema fields, but the shipped Active-carts widget surfaces **summed line-item quantity** (selected vs not-selected-for-checkout) as its primary figures, with `count` demoted to an internal denominator for `average`. Cart statistics are scoped to **exactly the requested `currencyCode`** — a customer's carts in other currencies are excluded outright, never folded or converted (unlike order-statistics Money, BL-SR-004). Gift line items are included in the item-quantity figures but excluded from the money total/count and from the storefront's own cart-page counter — a known, currently-unresolved inconsistency. Same `period`/`comparison` shape as order statistics; same creator+membership scope (BL-SR-002).
- **Verify:** Rep with active carts across served orgs → the `active-carts` period returns selected/unselected item quantities. A customer's carts in a currency other than the requested `currencyCode` contribute nothing to the figures (not converted, not merged).
- **Violation signal:** Wishlists or empty carts counted as active; carts in another currency folded into the requested-currency figures; another rep's carts included.
- **Agents:** qa-backend-expert
- **Docs:** N/A — pre-GA module, no VirtoOZ coverage (§1a).
- **Source:** vc-module-sales-rep `CustomerCartStatisticsService.BuildQuery` — `query.Where(x.Currency == currencyCode)` ("One cart per currency, mirrored on a switch, so folding every currency would report one cart as many"); `AddCartFiguresAsync` excludes `IsGift` from total/count while `AddItemQuantitiesAsync` does not.
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; DRIFT — Active-carts widget redesign, per-currency scoping, and the gift-item inconsistency. Docs N/A per §1a; Source + Live agree.)
- **Promoted:** 2026-07-23 (TLC-2026-07-23-1943); restored 2026-07-28.
