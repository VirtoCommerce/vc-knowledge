---
id: KB-7B4DC6DD
subject: On /account/orders/{id}, two configured lines of the same configurable product with different subscriptions both show the first subscription's number and frequency
plane: experiential
question: How does the order page label each line when one order started two subscriptions on the same configurable product?
questions:
  - text: Order page shows the wrong subscription number on a configured line
  - text: Two configured subscription lines in one order show the same SUB number
  - text: Is the subscription row on the order line matched by productId?
concepts:
  - id: configurable-product
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders/{id}
  - coordinate: Query.orderSubscriptionInfos
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 1dfb1b0bb5149a92
    conditions: theme=2.63.0-pr-2602-c8eb; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-cb70; store=B2B-store
    defect: suspected
    at: 2026-11-19T15:20:26.035Z
    by: session:d9b8e380
    who: kutasinaelena
---
One checkout with two subscribed configured lines of the same configurable product (Drop Bar every 2 weeks at 266; Standard every month at 228) created two Active subscriptions (Account > Subscriptions shows them correctly with their compositions and frequencies). On the order page both lines read 'Subscribed · Every month · Subscription <the monthly one>', including the Drop Bar line whose subscription is every 2 weeks. Query.orderSubscriptionInfos returns both subscriptions but keyed only by productId (no line-item id), so the page cannot tell the two lines apart and labels both with the first match.
