---
id: KB-EEB9B6FC
subject: "orderProductSubscriptionNow is idempotent under concurrency: two parallel calls give one renewal, one order, one authorization (the second is refused 'This subscription was just ordered'), and order-now during scheduler/run joins the scheduler's renewal"
plane: experiential
question: What happens when Mutation.orderProductSubscriptionNow is called twice in parallel or while scheduler/run is running?
questions:
  - text: Can a double click on Order now create two renewal orders?
  - text: Why does orderProductSubscriptionNow return SUBSCRIPTION_ERROR 'This subscription was just ordered'?
  - text: Order now right after checkout is refused?
status: active
appliesTo:
  - axis: module
    value: productsubscription
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.orderProductSubscriptionNow
  - coordinate: POST /api/product-subscription/scheduler/run
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d
    at: 2026-11-16T16:50:50.650Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-8177; store=B2B-store; payment=MockStoredCardPaymentMethod
    at: 2026-11-18T13:56:16.090Z
    by: session:d9b8e380
    bodyHash: 4fc839b5bb5c
    who: kutasinaelena
    note: "Admin REST POST /api/product-subscription/subscriptions/{id}/order-now seconds after the checkout order: 400 'This subscription was just ordered…'; the same call ~130 s after the checkout order created one manual renewal (Authorized with card 4242, RetryScheduled with soft-decline 9995)."
---
Customer token, pickup subscription paid with mock 4242. Within 2 minutes of the subscription's last order (including the checkout order) every orderProductSubscriptionNow returns errors[] in HTTP 200: SUBSCRIPTION_ERROR 'This subscription was just ordered. Check your orders before ordering again.' After that window, two parallel calls: first returns a manual renewal in state Authorized with one order, the second gets the same SUBSCRIPTION_ERROR; renewals search shows 1 renewal, 1 new order, 1 authorized payment. With nextOrderDate set within the reminder window and order-now fired together with POST scheduler/run: the scheduler created the renewal (Scheduled -> Notified, reminder sent), order-now marked the same renewal manual ('Order now requested') and placed one order with one authorized payment; no second renewal. nextOrderDate restarts from the order-now date + interval.
