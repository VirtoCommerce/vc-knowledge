---
id: KB-CCCDDDE6
subject: "Adding a MockStoredCardPaymentMethod card on /account/payment-methods: last4 0069 is refused with SAVED_PAYMENT_METHOD_ERROR \"Your card has expired.\" (UI shows a generic error); last4 3220 returns AUTHENTICATION_REQUIRED, the page opens a simulated 3-D Secure dialog, Approve re-sends and saves the card, Decline saves nothing; a past expiry date is blocked client-side"
plane: experiential
question: What happens when an expired or 3-D Secure test card is added from the storefront Payment methods page with the stored-card test method?
questions:
  - text: Does addSavedPaymentMethod accept an expired card?
  - text: Does saving a 3DS card from the account page require authentication?
  - text: What does the Payment methods page do on AUTHENTICATION_REQUIRED?
concepts:
  - id: card-payment
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/payment-methods
  - coordinate: Mutation.addSavedPaymentMethod
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; module:VirtoCommerce.Payment=3.1012.0-pr-81-8654; paymentMethod:MockStoredCardPaymentMethod=active,bound-to-store
    at: 2026-11-16T16:38:13.022Z
    by: session:d9b8e380
    who: kutasinaelena
---
On /account/payment-methods, Add a card -> "Credit card (test)" (MockStoredCardPaymentMethod) offers test-card chips and has no billing-address block. Mutation.addSavedPaymentMethod with last4 0069 (exp 12/29) returned errors[0] "Your card has expired.", code SAVED_PAYMENT_METHOD_ERROR, nothing saved; the form shows only the inline alert "Something went wrong. Please try again.". With last4 3220 and threeDsCompleted=false the server returned AUTHENTICATION_REQUIRED "Your bank needs you to confirm this card (3-D Secure)."; the page opened a "Confirm with your bank" dialog (simulated). Decline closed it with no card saved and no message; Approve re-sent the mutation and the card was saved Active, non-default. Typing an expiry 01/24 shows "Expiration date must be in the future" and keeps Save card disabled, so no request is sent. 4242 saves Active, non-default when "Use as my default card" is unchecked.
