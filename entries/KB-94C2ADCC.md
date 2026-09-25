---
id: KB-94C2ADCC
subject: a guest on /cart gets no card form for cart-payment gateways and the order is booked unpaid
plane: experiential
question: What happens when a guest pays by bank card on /cart with CyberSource or Authorize.Net?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.createOrderFromCart
  - coordinate: CartType.availablePaymentMethods
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:14:22.517Z
    by: session:memimpor
    who: Lenajava1
---
For an anonymous guest, the cart-embedded card form of allowCartPayment gateways (CyberSource, Authorize.Net) does not render; only signed-in users get it. Place order still enables, and the order is created with status Payment required, isApproved false and no request to the gateway - payment is settled afterwards. This is the intended guest path (BY DESIGN), not a payment bypass: the storefront shows the card form and gates Place order on it only when the user is authenticated. An unpaid Payment required order from the cart card path is therefore not evidence of a bypass on its own.
