---
id: KB-44CA8C9C
subject: A subscribed configured cart line loses its subscription discount while a one-time line of the same configurable product is in the cart
plane: experiential
question: Does the subscription price stay on a subscribed configurable cart line when the cart also holds a one-time line of the same configurable product?
questions:
  - text: Why does a Subscribed configured cart line show the one-time price?
  - text: Subscribe & Save discount missing on /cart for a configurable product with two configurations
  - text: "Mixed subscribed and one-time lines of one configurable product: pricing"
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.cart
  - coordinate: Mutation.setCartItemSubscription
  - coordinate: Query.cartSubscriptionItems
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 1dfb1b0bb5149a92
    conditions: theme=2.63.0-pr-2602-c8eb; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-cb70; store=B2B-store
    defect: suspected
    at: 2026-11-19T15:20:07.429Z
    by: session:d9b8e380
    who: kutasinaelena
    note: "distinct: Neither entry covers pricing of two lines of one configurable product with mixed purchase modes; this is the discount being dropped from the subscribed line."
---
USER cart, B2B-store, plan Percent 5. Configurable product added from the PDP with option Drop Bar as Subscribe & Save: line 266 (280 x 0.95), Subscribed · Every 2 weeks. Then the same configurable product added again with option Standard as One-time (a separate line, 240). After that the subscribed Drop Bar line is priced 280 (no subscription discount) while GetCartSubscriptionItems still returns its subscription item (WEEK/2) and the line still shows Subscribed; persists after reload. Subscribing the Standard line too restores both: 266 + 228. So a mixed cart charges the one-time price on a line the buyer subscribed at the discounted price.
