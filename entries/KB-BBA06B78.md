---
id: KB-BBA06B78
subject: On the PDP, a second Increase quantity on a product already in the cart is sent as one batched quantity update about 10 seconds later, while the "in Cart" count moves at once
plane: experiential
question: When does the storefront product page send the quantity change for a second Increase quantity click on a product that is already in the cart?
questions:
  - text: Why does the cart on the server still show quantity 1 right after I clicked + twice on the product page?
  - text: Is the PDP quantity stepper's update debounced or batched?
  - text: How long after clicking + on the product page does the cart update reach the server?
  - text: Why does the in Cart badge show 2 but getCart returns 1?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /graphql
  - coordinate: Mutation.updateCartQuantity
  - coordinate: /product/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2647; module:VirtoCommerce.XCart=3.1046.0; store=B2B-store; role=anonymous
    at: 2026-11-12T10:50:28.178Z
    by: session:517b1f88
    who: Lenajava1
---
Anonymous session, store B2B-store, a simple in-stock product (SKU 201482) opened from search. The first Increase quantity click added the line at once (one GraphQL mutation, analytics add_to_cart qty 1). A second Increase quantity click about 2 s later changed the buy box at once — spinbutton 2, "in Cart" 2 — but no GraphQL request followed for several seconds; the next cart mutation went out about 10 s after the click (analytics update_cart_item, previous_quantity 1, new_quantity 2). So a getCart read made right after the second click still returns quantity 1. A test that clicks + more than once must wait for that mutation response, not for the count label or a short fixed delay.
