---
id: KB-D456FA1E
subject: "Product-subscription xAPI refuses another customer's subscription, renewal and card: Forbidden for subscriptions/renewals, 'The card was not found' for cards, authorizePayment with someone else's storedPaymentMethodId isSuccess false; anonymous gets Unauthorized"
plane: experiential
question: Can a signed-in customer read or change another customer's product subscription, renewal or saved card through xAPI?
questions:
  - text: What does productSubscription(id) return for a subscription of another customer?
  - text: Can authorizePayment use another customer's storedPaymentMethodId?
  - text: What do savedPaymentMethods and productSubscriptions return anonymously?
status: active
appliesTo:
  - axis: module
    value: productsubscription
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.productSubscription
  - coordinate: Query.subscriptionRenewal
  - coordinate: Mutation.changeProductSubscription
  - coordinate: Mutation.cancelProductSubscription
  - coordinate: Mutation.removeSavedPaymentMethod
  - coordinate: Mutation.setDefaultSavedPaymentMethod
  - coordinate: Mutation.authorizePayment
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; payment=MockStoredCardPaymentMethod
    at: 2026-11-16T17:30:03.063Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-8177; module:VirtoCommerce.XCart=3.1049.0-alpha.318-vcst-6507; module:VirtoCommerce.Payment=3.1012.0-alpha.334-vcst-6507
    at: 2026-11-18T10:32:27.159Z
    by: session:d9b8e380
    bodyHash: 9d61bf0ef7aa
    who: kutasinaelena
    note: "Re-observed on the newer build: another customer's card id -> removeSavedPaymentMethod / setDefaultSavedPaymentMethod 'The card was not found.' SAVED_PAYMENT_METHOD_ERROR, owner's cards unchanged; anonymous savedPaymentMethods, cardPaymentMethods, addSavedPaymentMethod, removeSavedPaymentMethod, setDefaultSavedPaymentMethod, productSubscriptions, orderSubscriptionInfos all Unauthorized."
---
With a B2B customer token (and a second, unrelated customer), on other customers' objects (one in another organization): Query.productSubscription, Query.subscriptionRenewal, Mutation.changeProductSubscription and cancelProductSubscription -> HTTP 200, errors[] 'Access denied.' code Forbidden, data null. removeSavedPaymentMethod / setDefaultSavedPaymentMethod on another customer's card -> errors[] 'The card was not found.' code SAVED_PAYMENT_METHOD_ERROR (does not reveal that the card exists). authorizePayment on the caller's own unpaid order with parameters storedPaymentMethodId = another customer's card -> data.authorizePayment {isSuccess:false, errorMessage:"This saved card can't be used. Choose another card."}, payment stays New. Admin re-read before/after: subscriptions, cards and the order unchanged. Without a token every one of these plus savedPaymentMethods, productSubscriptions and addSavedPaymentMethod -> errors[] 'Anonymous access denied or access token has expired or is invalid.' code Unauthorized.
