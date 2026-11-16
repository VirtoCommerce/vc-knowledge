---
id: KB-67CE3EC1
subject: Paying a subscription order with a card that needs 3-D Secure on save shows a bank confirmation dialog; the card is not saved and the subscription stays PendingActivation until the buyer approves
plane: experiential
question: What happens to the saved card and the subscription when the first subscription payment needs 3-D Secure?
questions:
  - text: Is a card saved before 3-D Secure is confirmed on a subscription order?
  - text: Is a subscription Active while its first payment waits for 3DS?
  - text: Does paying a second subscription order with the same saved card create a duplicate saved card?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /checkout/payment
  - coordinate: Mutation.authorizePayment
  - coordinate: Query.savedPaymentMethods
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store; payment=MockStoredCardPaymentMethod
    at: 2026-11-16T16:25:23.093Z
    by: session:d9b8e380
    who: kutasinaelena
---
MockStoredCardPaymentMethod, /checkout/payment, 'Use a new card' with the 3DS-at-checkout test card (last4 3220): Pay opens a 'Confirm with your bank' dialog (Decline / Approve). While it is open, Query.savedPaymentMethods still returns only the previous card and the new subscription is PendingActivation. After Approve: /checkout/payment/success, a second saved card (last4 3220, not default) appears, and the subscription is Active and linked to that new card. Separately, a later subscription order paid with the already-saved default card (4242) left savedPaymentMethods at one 4242 card - no duplicate. SavedPaymentMethod exposes brand, last4, expiry, cardholderName, status, isDefault, isExpired, createdDate - no token or PAN, and no last-used field.
