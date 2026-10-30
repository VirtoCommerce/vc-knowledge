---
id: KB-EFCFCEE0
subject: On the storefront product page, an anonymous shopper's first Increase quantity click adds the item to the cart through Mutation.updateCartQuantity, and that response already lists the settled line item
plane: experiential
question: How does the product page add an item to the cart for an anonymous shopper, and does the mutation's own response list the new line?
questions:
  - text: Which GraphQL mutation does the product page quantity stepper send?
  - text: Can an anonymous shopper add to cart from /product/{id} without signing in?
  - text: Does updateCartQuantity return the line item id right after adding?
status: active
appliesTo:
  - axis: actor
    value: anonymous
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /product/{id}
  - coordinate: Mutation.updateCartQuantity
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: theme=2.61.0; store=B2B-store; currency=USD
    at: 2026-10-30T17:09:46.923Z
    by: session:b942088e
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: theme=2.61.0; store=B2B-store; currency=USD
    at: 2026-10-30T17:15:34.571Z
    by: session:b942088e
    bodyHash: 35df019b2f5a
    who: Lenajava1
    note: "Re-observed on a simple product PDP reached via a top-level category: one Increase quantity click sent Mutation.updateCartQuantity, HTTP 200, no errors[], response carried itemsQuantity 1 and the line; PDP showed \"in Cart 1\" and header Cart badge 1."
---
Anonymous session, store B2B-store, USD, en-US. Opened /product/{id} for an in-stock simple product (stock count shown next to an In stock label; quantity stepper at 0, Decrease disabled). One click on Increase quantity sent operation UpdateShortCartItemQuantity = Mutation.updateCartQuantity with command.items [{productId, quantity 1, itemCurrencyCode USD}] and the anonymous userId. HTTP 200, no errors[]; the response returned the cart id, itemsQuantity 1 and items[] containing the new line (id, sku, quantity 1, productId). The stepper then read 1 and an "in Cart 1" badge appeared beside the stock count. No separate Add to cart button and no sign-in were needed. Unlike addItem (see KB-A613223B), this path's own response carried the line.
