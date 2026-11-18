---
id: KB-B3443FAD
subject: With store setting ProductSubscription.Enabled=false the storefront shows no subscription UI, sends no subscription GraphQL operations and 404s /account/subscriptions; the switch takes effect within a minute of a full-store PUT and reverts the same way
plane: experiential
question: What does the storefront show when ProductSubscription.Enabled is turned off for the store, and how fast does it take effect?
questions:
  - text: Does turning off product subscriptions for a store hide Subscribe & Save on the PDP and cart?
  - text: Is /account/subscriptions still reachable when the subscription feature is off?
  - text: Is there a cache delay after changing ProductSubscription.Enabled through PUT /api/stores?
status: active
appliesTo:
  - axis: feature
    value: product-subscriptions
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: PUT /api/stores
  - coordinate: /account/subscriptions
  - coordinate: Query.subscriptionOffers
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store; setting:ProductSubscription.Enabled=false (store)
    at: 2026-11-16T17:45:17.192Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: theme=2.63.0-pr-2602-6507-6507ff26; store=B2B-store; setting:ProductSubscription.Enabled=false (store, explicit)
    at: 2026-11-18T11:27:45.712Z
    by: session:d9b8e380
    bodyHash: 3d312511cbb4
    who: kutasinaelena
    note: "Re-seen 2026-11-18 as guest and signed-in buyer: PDP no purchase options, cart line no Subscribe (Save for later only), no terms block, no Subscriptions menu entry, /account/subscriptions 404, HAR with no subscription GraphQL ops, no errors[] and no 4xx/5xx. After setting it back to true, a new session showed the PDP radios within ~2 min and GetSubscriptionOffers/GetCartSubscriptionItems were sent again."
---
B2B-store, theme 2.63.0-pr-2602-399d, signed-in buyer, new browser session each time. Store setting ProductSubscription.Enabled (store value null = inherits default true) set to false through GET /api/stores/{id} + full-body PUT /api/stores (204, read back false). ~30 s later: PDP of a product with an active PERCENT plan showed only Price $35.00 and the quantity stepper, no One-time / Subscribe & Save radios; /cart line had no Subscribe & Save checkbox or frequency and no 'Your subscription' terms block; with address, delivery and a non-card payment (Cash) Place order was enabled, and the payment list was the full store list (Apple Pay, Stripe, Cash, Points, Credit card (test), Barion) instead of card-only; the account sidebar had no 'Subscriptions' entry and /account/subscriptions rendered the 404 page. 'Payment methods' (/account/payment-methods, saved cards) stayed in the menu and worked. HAR of the whole walk: no 4xx/5xx from the storefront or platform, no GraphQL errors[], and none of GetSubscriptionOffers / GetCartSubscriptionItems / GetSubscriptionTerms was sent. Setting it back to true the same way: a new session ~30 s later showed the PDP radios, the Subscriptions menu entry and the list again. No Admin store save or restart needed in either direction.
