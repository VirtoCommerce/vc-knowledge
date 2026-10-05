---
id: KB-C7BFA751
subject: the Authorize.Net card form takes expiry as MM/YY and works only in USD
plane: experiential
question: What expiry format does the Authorize.Net card form on /cart accept, and does it work in non-USD currencies?
questions:
  - text: Why does my card expiry change to a past date when I type the full year?
  - text: Does this card processor's inline form check for an expired date before submitting?
  - text: What expiry mask does this processor's inline card form on the cart page apply?
  - text: Is this card processor expected to fail or be unavailable for a non-USD cart?
  - text: Does the USD-only restriction apply to the other card processors too?
concepts:
  - id: card-payment
  - id: currency
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.initializeCartPayment
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:31.965Z
    by: session:memimpor
    who: Lenajava1
---
The Authorize.Net inline card form on /cart masks expiry as MM/YY, like the other inline card forms. Typing a four-digit year is truncated by the mask - 12/2029 becomes 12/20, an expired date - and the form does no client-side expired-date check, so an expired card can be submitted. The Authorize.Net integration is set up for USD only: unavailability or rejection in a non-USD cart is by design, and the constraint does not apply to the other processors.
