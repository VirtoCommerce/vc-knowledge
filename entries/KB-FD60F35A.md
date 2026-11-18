---
id: KB-FD60F35A
subject: A subscription order paid with an already-saved hard-decline mock card (last4 0021) stayed PendingActivation although its payment was Authorized; the same card entered as a new card activated the subscription
plane: experiential
question: Why does a product subscription stay PendingActivation after authorizePayment succeeds with a saved MockStoredCardPaymentMethod card?
status: active
appliesTo:
  - axis: module
    value: productsubscription
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.authorizePayment
  - coordinate: POST /api/product-subscription/subscriptions/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-8177; store=B2B-store; payment=MockStoredCardPaymentMethod
    at: 2026-11-18T13:56:27.793Z
    by: session:d9b8e380
    who: kutasinaelena
---
Buyer token, xAPI chain cart -> setCartItemSubscription -> acceptSubscriptionTerms -> createOrderFromCart -> authorizePayment with storedPaymentMethodId of a saved Active card last4 0021 (the buyer also held other 0021 cards in NeedsUpdate/Revoked from earlier renewals). authorizePayment isSuccess=true and the order's in-payment was Authorized, but the subscription was still PendingActivation minutes later. Repeating with 0021 entered as a NEW card (storeForFutureUse, another expiry year) gave an Active subscription within seconds. Saved 4242/9995/3184 cards activated normally. A seeder that needs an Active subscription on 0021 should pay with a new card.
