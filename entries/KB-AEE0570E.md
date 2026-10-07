---
id: KB-AEE0570E
subject: createOrderFromCart and createQuoteFromCart share one cached CartAggregate per cart, so validation results cached by a failed order or quote attempt are reused by the next attempt until the cart is saved
plane: experiential
question: Does Mutation.createOrderFromCart reuse a cached CartAggregate validation result from an earlier createQuoteFromCart or createOrderFromCart call on the same cart?
status: active
appliesTo:
  - axis: module
    value: x-cart
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Mutation.createOrderFromCart
  - coordinate: Mutation.createQuoteFromCart
  - coordinate: Query.cart
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-07T14:54:58.352Z
    by: session:123fed0f
    who: yuskithedeveloper
---
Observed 3/3 runs on XCart 3.1040.0-alpha.294 (VCST-6089 fix build), XOrder 3.1015.0-pr-53, Quote 3.1004.0-pr-158. Cart with one line item whose product was then deleted from the catalog (xAPI loads cart products by ObjectIds, which skips the status:visible filter, so only a deleted product counts as missing; a merely inactive one is still found). Sequence: any cart mutation (cache eviction) -> createOrderFromCart rejected -> createQuoteFromCart rejected (its own line-item check) -> product re-created with the same id -> Query.cart validationErrors(ruleSet:"*") returns [] -> createOrderFromCart STILL rejected "The cart has validation errors" -> changeComment (cart save) -> createOrderFromCart succeeds. So the order gate reads the "*" result cached on the aggregate instance by the earlier failed attempt; the instance is the IPlatformMemoryCache entry keyed (cartId, invariant culture, response group Full, no product fields), which both handlers load (GetCartByIdQuery / GetCartForShoppingCartAsync); since XCart 3.1039.0 the response group is normalised so the two share it. The GraphQL cart query uses response group Full minus RecalculateTotals, a different entry: its validationErrors(ruleSet:"*") showed CART_PRODUCT_UNAVAILABLE while a createOrderFromCart right after (product restored) succeeded, i.e. cart-query validation never reaches the order/quote gate. Any cart save expires all entries of that cart. Catalog changes do not, so a checkout retry after the catalog is fixed fails until the cart changes.
