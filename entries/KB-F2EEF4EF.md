---
id: KB-F2EEF4EF
subject: Order history marks only renewal orders with a 'Subscription' badge; the order page shows a subscription row with a SUB link under each subscribed line only
plane: experiential
question: Which orders and order lines does the storefront mark as subscription in order history and on the order page?
questions:
  - text: Does the initial subscription order get a Subscription badge in /account/orders?
  - text: Where does the SUB link on the order page lead?
  - text: Is a one-time line in a subscription order marked on the order page?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders
  - coordinate: /account/orders/{id}
  - coordinate: /account/subscriptions
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: theme=2.63.0-pr-2602-6507ff26; store=B2B-store; payment=MockStoredCardPaymentMethod
    at: 2026-11-18T10:40:58.376Z
    by: session:d9b8e380
    who: kutasinaelena
---
Theme 2.63.0-pr-2602 (with vc-frontend #2671), B2B-store, signed-in buyer. Placed one order with PEPSI subscribed monthly + LIPTON subscribed weekly + OREO one-time, paid with Credit card (test) 4242. /account/orders: the new (initial) order has no badge; orders placed by Order now / the scheduler carry a 'Subscription' badge whose title reads 'Placed automatically by your subscription for the delivery of <date>'. /account/orders/<id> of the initial order: each subscribed line shows 'Subscribed · Every <interval>' and a link 'Subscription SUB…' to /account/subscriptions?subscription=<subscriptionId>, which opens the Subscriptions page scrolled to that delivery card; the one-time OREO line shows no row; no 'placed automatically' alert on the initial order. Source: order-line-subscription.vue matches the subscription by order id + productId.
