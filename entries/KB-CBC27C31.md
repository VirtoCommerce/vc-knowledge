---
id: KB-CBC27C31
subject: "On ProductSubscription 3.1000.0-pr-1-8177 the customer Order now and Skip actions are off by default (ProductSubscription.Customer.AllowOrderNow / AllowSkip default false): orderProductSubscriptionNow returns SUBSCRIPTION_ACTION_DISABLED and canOrderNow / canSkip are false"
plane: experiential
question: Why does orderProductSubscriptionNow return SUBSCRIPTION_ACTION_DISABLED and canOrderNow false for every subscription?
questions:
  - text: Are customer Order now and Skip enabled by default for product subscriptions?
  - text: What does SUBSCRIPTION_ACTION_DISABLED mean on a product subscription mutation?
status: active
appliesTo:
  - axis: module
    value: productsubscription
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.orderProductSubscriptionNow
  - coordinate: Query.productSubscriptions
  - coordinate: POST /api/product-subscription/subscriptions/{id}/order-now
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-8177; setting:ProductSubscription.Customer.AllowOrderNow=unset(default false); setting:ProductSubscription.Customer.AllowSkip=unset(default false)
    at: 2026-11-18T10:32:27.344Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: setting:ProductSubscription.Customer.AllowSkip=false (store B2B-store explicit); setting:ProductSubscription.Customer.AllowOrderNow=false (store B2B-store explicit)
    at: 2026-11-18T15:15:25.098Z
    by: session:d9b8e380
    bodyHash: 652746d3a862
    who: kutasinaelena
    note: "2026-11-18 with explicit store false: canSkip/canOrderNow false; skipProductSubscription and orderProductSubscriptionNow both SUBSCRIPTION_ACTION_DISABLED in errors[] (HTTP 200); nextOrderDate and skipCount unchanged."
  - method: observation
    deployment: vcptcore_qa1
    build: 6c2522652e022adc
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-a88d; setting:ProductSubscription.Customer.AllowSkip=false (store B2B-store explicit); setting:ProductSubscription.Customer.AllowOrderNow=false (store B2B-store explicit)
    at: 2026-11-19T11:04:08.121Z
    by: session:d9b8e380
    bodyHash: 652746d3a862
    who: kutasinaelena
    note: "2026-11-19 with explicit store false: productSubscription canSkip/canOrderNow/canChangePaymentMethod false (canPause/canCancel/canChange true); skipProductSubscription SUBSCRIPTION_ACTION_DISABLED, nothing changed. Defaults (unset) not re-checked."
---
With the store and global values of ProductSubscription.Customer.AllowOrderNow and AllowSkip unset (GET /api/platform/settings/<name> -> value null, defaultValue false; AllowPause default true), a buyer's Mutation.orderProductSubscriptionNow returns HTTP 200 errors[] "This store doesn't let customers do this. Please contact the store." code SUBSCRIPTION_ACTION_DISABLED, and Query.productSubscriptions reports canOrderNow:false, canSkip:false on every subscription (canChangeFrequency true). The admin REST POST /api/product-subscription/subscriptions/{id}/order-now is not gated by it; right after the initial order it answers 400 "This subscription was just ordered. Check your orders before ordering again." (two parallel calls both refused, no order created).
