---
id: KB-4C7F5FF4
subject: skipProductSubscription takes only the subscription id, so a Skip pressed on a stale subscription page skips whatever delivery is next on the server, not the date that page shows
plane: experiential
question: What happens when a buyer presses Skip next delivery on a subscription page opened before another tab already skipped?
questions:
  - text: two tabs skip the same subscription
  - text: is skipProductSubscription idempotent
  - text: does Skip next delivery check which date it skips
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/subscriptions/{id}
  - coordinate: Mutation.skipProductSubscription
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d
    defect: suspected
    at: 2026-11-16T16:17:25.643Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: platform=3.1099.0; theme=2.63.0-pr-2602-399d; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; store=B2B-store
    at: 2026-11-16T18:40:26.051Z
    by: session:d9b8e380
    bodyHash: 74cba4b9c8eb
    defect: suspected
    who: kutasinaelena
    note: "Fresh repro on a new monthly subscription: Dec 16 → Jan 16 (tab 2) → Feb 16 (stale tab 1), skipCount 2, variables only {subscriptionId}. Source: InputProductSubscriptionId has only subscriptionId; SubscriptionManager.SkipNextAsync adds one interval to the stored NextOrderDate with no expected-date check. A double click on Skip in one fresh tab sends exactly one mutation."
  - method: observation
    deployment: vcptcore_qa1
    build: 959440e006132a35
    conditions: platform=3.1099.0; theme=2.63.0-pr-2602-57ad; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-79cf; store=B2B-store
    at: 2026-11-17T12:19:53.141Z
    by: session:d9b8e380
    bodyHash: 74cba4b9c8eb
    who: kutasinaelena
    contradicts: true
    contested: theme 2.63.0-pr-2602-57ad vs 2.63.0-pr-2602-399d; module:VirtoCommerce.ProductSubscription 3.1000.0-pr-1-79cf vs 3.1000.0-pr-1-595d; theme 2.63.0-pr-2602-0666-066611ae vs 2.63.0-pr-2602-399d-399d5bf8
    note: "Fixed on the newer build. skipProductSubscription now takes InputSkipProductSubscription {subscriptionId, expectedNextOrderDate}; the storefront sends the date the page shows. A Skip from a stale tab (other tab already skipped) is refused with SUBSCRIPTION_ERROR \"The next delivery has changed since this page was opened…\", the toast says the page was refreshed, skipCount does not move. Via xAPI: stale date refused, correct date skips once, replay refused. Caveat: omitting expectedNextOrderDate (or null) still skips unguarded."
---
Active monthly subscription with next delivery Dec 16, 2026, open in two tabs of /account/subscriptions/{id}. Tab 2: Skip next delivery, then next = Jan 16, 2027. Tab 1 (still showing Dec 16): Skip next delivery, then next = Feb 16, 2027, with the toast "Next delivery skipped.". The mutation variables are only {subscriptionId}: no expected date or version is sent, and no confirmation dialog is shown. Two deliveries were skipped where the buyer of tab 1 saw only one. Double clicks on Order now confirm, Place order and Cancel confirm each sent exactly one mutation.
