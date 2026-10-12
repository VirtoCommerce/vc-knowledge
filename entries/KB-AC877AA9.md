---
id: KB-AC877AA9
subject: A cart line whose product was deleted is flagged CART_PRODUCT_UNAVAILABLE under validationErrors ruleSet "*" but not under ruleSet "default"
plane: experiential
question: Does Query.cart validationErrors(ruleSet:"default") report a line item whose product was deleted, or only ruleSet "*"?
status: active
appliesTo:
  - axis: module
    value: x-cart
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Query.cart
  - coordinate: Mutation.createQuoteFromCart
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T15:26:21.029Z
    by: session:df40a5cc
    who: Lenajava1
---
The cart had one line item, a valid FixedRate shipment and a manual payment. Before the product was deleted, validationErrors(ruleSet:"*") returned []. After the product was deleted through REST and was gone from xAPI product(id), ruleSet "*" returned one CART_PRODUCT_UNAVAILABLE ("The product is no longer available for purchase", objectType LineItem), while ruleSet "default" returned []. This held 2/2 runs. So the "default" ruleSet (the one createQuoteFromCart validates with) does not catch a deleted product, and createQuoteFromCart's rejection of such a cart comes from its own line-item check.
