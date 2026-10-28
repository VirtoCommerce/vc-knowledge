---
id: KB-94C2ADCC
subject: a guest on /cart gets no card form for cart-payment gateways and the order is booked unpaid
plane: experiential
question: What happens when a guest pays by bank card on /cart with CyberSource or Authorize.Net?
questions:
  - text: Checking out as a guest, why is there no box to type my card details even though card payment is selected?
  - text: Is an unpaid Payment required order placed by a guest through a card gateway evidence of a payment bypass?
  - text: Does the cart-embedded card form of cart-payment gateways render for anonymous users, and does Place order stay enabled?
  - text: What status and approval flag does createOrderFromCart give a guest order paid by card, and is the gateway called?
concepts:
  - id: card-payment
  - id: checkout
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
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    at: 2026-10-28T11:12:27.048Z
    by: session:7e8ea7fb
    bodyHash: 47a8df1639dc
    who: Lenajava1
    note: "Signed-in user: card form renders on /cart once a line item exists and Bank card (CyberSource) is selected."
---
For an anonymous guest, the cart-embedded card form of allowCartPayment gateways (CyberSource, Authorize.Net) does not render; only signed-in users get it. Place order still enables, and the order is created with status Payment required, isApproved false and no request to the gateway - payment is settled afterwards. This is the intended guest path (BY DESIGN), not a payment bypass: the storefront shows the card form and gates Place order on it only when the user is authenticated. An unpaid Payment required order from the cart card path is therefore not evidence of a bypass on its own.
