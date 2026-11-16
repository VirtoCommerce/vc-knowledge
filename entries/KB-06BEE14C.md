---
id: KB-06BEE14C
subject: Store overrides ProductSubscription.Terms.Version/Text are shown in the /cart terms block and recorded on the consent (termsVersion, termsText, new termsHash) for every buyer of that store from the next cart load
plane: experiential
question: Does a store-level ProductSubscription.Terms.Version/Text override change the terms shown at checkout and the version stored on the subscription consent?
questions:
  - text: What termsVersion does GET /api/product-subscription/consents/{id} hold after the store terms are overridden?
  - text: Is the subscription terms text on /cart taken from the store setting?
status: active
appliesTo:
  - axis: feature
    value: product-subscriptions
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/product-subscription/consents/{id}
  - coordinate: /cart
  - coordinate: Mutation.acceptSubscriptionTerms
  - coordinate: PUT /api/stores
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store; payment=MockStoredCardPaymentMethod; setting:ProductSubscription.Terms.Version=2026-11-test (store)
    at: 2026-11-16T17:45:36.416Z
    by: session:d9b8e380
    who: kutasinaelena
---
B2B-store, theme 2.63.0-pr-2602-399d. Store settings ProductSubscription.Terms.Version and .Terms.Text (both null = module defaults, version 2026-10) set via full-store PUT /api/stores to '2026-11-test' and an AGENT-TEST text. ~1 min later, a buyer in a new session put a PERCENT-plan product on Subscribe & Save: /cart 'Your subscription' block showed exactly the override text; Place order disabled until 'I agree...' ticked (Mutation AcceptSubscriptionTerms). Order paid with the saved mock card (payment Authorized), subscription Active. GET /api/product-subscription/consents/{subscription.consentId}: termsVersion '2026-11-test', termsText = override, termsHash 99b6f233... (differs from the 2026-10 hash), channel storefront, objectType ShoppingCart, purpose subscription, storedPaymentMethodId null. A second buyer of the same store checking out in that window also got termsVersion 2026-11-test — the override applies store-wide immediately, so it must not be flipped while others test the store. Writing value "" to both settings read back null (itHasValues false).
