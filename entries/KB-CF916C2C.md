---
id: KB-CF916C2C
subject: Switching the storefront header currency re-prices the whole cart; a mixed-currency cart ("Products in {code}" group on /cart) is only reachable via xAPI addItem with itemCurrencyCode
plane: experiential
question: How do I get a cart line in another currency so /cart shows a "Products in EUR" group?
questions:
  - text: Does changing currency in the storefront header convert existing cart lines?
  - text: Can a USD cart hold an EUR line item?
  - text: How to test the other-currency group on the cart page
status: active
appliesTo:
  - axis: currency
    value: usd
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
  - axis: user
    value: anonymous
anchors:
  - coordinate: /cart
  - coordinate: Mutation.addItem
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: theme=2.62.0-pr-2640-a053-a053ca3c; store=B2B-store
    at: 2026-11-09T11:49:41.226Z
    by: session:a835884e
    who: Aleksandra-Mitricheva
---
Anonymous, B2B-store, theme 2.62.0. With a USD cart, switching the header currency to EUR re-prices every line in EUR (no separate group); adding a product while in EUR and switching back to USD turns it into a USD line too, so the UI alone never produces a mixed-currency cart. Sending xAPI Mutation.addItem on the same anonymous USD cart (storeId, userId, currencyCode USD) with itemCurrencyCode "EUR" creates a line whose LineItemType.currencyCode is EUR while cart.currency stays USD; /cart then renders it in a separate "Products in EUR" section after the vendor groups, with its own select-all and an EUR subtotal. The product's own min/max quantity still applies (Epson WorkForce WF-3640 needed qty 11; qty 1 returned PRODUCT_MIN_MAX_QTY and no line).
