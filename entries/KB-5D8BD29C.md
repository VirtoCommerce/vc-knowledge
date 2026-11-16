---
id: KB-5D8BD29C
subject: Removing the default saved card revokes it (status Revoked, dropped from savedPaymentMethods) and promotes the most recently created active card to default; setDefaultSavedPaymentMethod keeps exactly one default
plane: experiential
question: Which card becomes the default when the customer removes their default saved card on /account/payment-methods?
questions:
  - text: Are revoked saved cards still listed in savedPaymentMethods?
  - text: Does Make default leave more than one default card?
  - text: What status does a removed saved payment method get?
concepts:
  - id: payment-method
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/payment-methods
  - coordinate: Mutation.removeSavedPaymentMethod
  - coordinate: Mutation.setDefaultSavedPaymentMethod
  - coordinate: Query.savedPaymentMethods
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; module:VirtoCommerce.Payment=3.1012.0-pr-81-8654; paymentMethod:MockStoredCardPaymentMethod=active,bound-to-store
    at: 2026-11-16T16:38:12.846Z
    by: session:d9b8e380
    who: kutasinaelena
---
Multi-org B2B buyer, B2B-store, MockStoredCardPaymentMethod cards. Cards: 3220 created 16:08 (made default via Make default = Mutation.setDefaultSavedPaymentMethod), 4242 #1 created 16:30:04 (pays an Active subscription), 4242 #2 created 16:34:38. After set-default the following Query.savedPaymentMethods held exactly one isDefault=true. Remove on the default 3220 -> Mutation.removeSavedPaymentMethod returned it with status "Revoked", isDefault false; the refreshed savedPaymentMethods no longer contains it and 4242 #2 (the newest active card) is isDefault=true, the page shows its Default badge. Removing a free non-default card likewise returns status Revoked and drops it from the list. The list orders the default card first, then by newest.
