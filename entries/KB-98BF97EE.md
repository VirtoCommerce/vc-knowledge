---
id: KB-98BF97EE
subject: On the product page an out-of-stock product that has a subscription plan shows no purchase options (Subscribe & Save) block, only the price and a Stock alert button
plane: experiential
question: Does the PDP show the Subscribe & Save purchase options for a product with a subscription plan when the product is out of stock?
questions:
  - text: Why is the Subscribe & Save block missing on a product page?
  - text: Can an out-of-stock product be subscribed to from the storefront?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /product/{id}
  - coordinate: /bulk-order
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store
    at: 2026-11-16T15:57:02.302Z
    by: session:d9b8e380
    who: kutasinaelena
---
Signed-in buyer, Chromium 1920, theme 2.63.0-pr-2602-399d, B2B-store. A product with a PERCENT 10 subscription plan (subscriptionOffers returns its offer) but inventory 0 at its only fulfillment center (isAvailable false, no backorder/preorder): /product/{id} sidebar shows only "Price: $7.50" and a Stock alert button — no Add to cart, no quantity stepper, no One-time / Subscribe & Save options. Adding the same SKU through /bulk-order is refused with "The product is out of stock". So a subscription cannot be started on a product that cannot be bought; a run that needs the Subscribe & Save block needs stock on the plan's product first.
