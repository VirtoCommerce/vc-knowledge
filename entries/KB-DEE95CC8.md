---
id: KB-DEE95CC8
subject: On /account/subscriptions/{id} skip moves the next date by one interval, but saving a new frequency recomputes the next date from today and drops the skip; Order now only schedules a manual renewal, it does not create an order immediately
plane: experiential
question: How do skip, change frequency, pause, resume, Order now and cancel behave on the storefront subscription detail page?
questions:
  - text: Does changing a subscription's frequency keep a skipped delivery?
  - text: Does Order now on a product subscription place an order right away?
  - text: What quantity and date validation does the subscription edit form apply?
  - text: Does cancelling a subscription remove its open renewal?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/subscriptions/{id}
  - coordinate: Mutation.changeProductSubscription
  - coordinate: Mutation.skipProductSubscription
  - coordinate: Mutation.orderProductSubscriptionNow
  - coordinate: Mutation.cancelProductSubscription
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store
    at: 2026-11-16T16:25:23.246Z
    by: session:d9b8e380
    who: kutasinaelena
---
Theme 2.63.0-pr-2602. Subscription every 10 days, next Nov 26: 'Skip next delivery' -> Dec 6 (+1 interval), toast 'Next delivery skipped.', skipCount 1. Then changing Frequency to 'Every week' and Save -> next date became Nov 23 (today + 1 week), the skip was lost. Quantity 0 -> 'The minimum quantity is 1.', 100000 -> 'The maximum quantity is 9999.', Save disabled. A past date -> 'Date must be on or after 2026-11-17' (ISO date while the field shows MM/DD/YYYY). Quantity 2, date 12/10/2026 and card change persisted after reload. Pause offers For 2/4/8 weeks or 'Until I resume it'; paused shows 'No upcoming deliveries'; Resume -> Active, next date kept (Dec 10). 'Order now' dialog says 'We'll place an order ... right away and charge your saved card' and the toast says 'Order placed.', but the API shows only a manual SubscriptionRenewal in state Scheduled (cutoff = now, previewTotal 0, orderNumber null) and no order 6+ minutes later; /account/subscriptions then lists it under 'Upcoming deliveries'. Cancel subscription (optional reason) -> Cancelled at once, the open renewal is gone from subscriptionRenewals; nextOrderDate keeps its old value.
