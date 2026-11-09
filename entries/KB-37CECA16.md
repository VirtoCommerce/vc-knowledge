---
id: KB-37CECA16
subject: addOrUpdateCartPayment with a gateway the store does not have active returns a bare VALIDATION "Error trying to resolve field" at HTTP 200, the same as for a nonexistent gateway code
plane: experiential
question: Why does Mutation.addOrUpdateCartPayment with paymentGatewayCode LoyaltyPaymentMethod return "Error trying to resolve field" with code VALIDATION?
questions:
  - text: What does addOrUpdateCartPayment return for an inactive payment method?
  - text: Can a cart payment be set to LoyaltyPaymentMethod when the store has not enabled it?
status: active
appliesTo:
  - axis: module
    value: x-cart
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Mutation.addOrUpdateCartPayment
  - coordinate: POST /api/payment/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 0a0b8dac8ab16bcc
    conditions: platform=3.1076.8; module:VirtoCommerce.XCart=3.1039.0; module:VirtoCommerce.Loyalty=3.1009.0; setting:Loyalty.Mode=Mixed Cart
    at: 2026-11-09T00:20:22.512Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
vcptcore_regression, B2B-store, buyer token, a cart with one USD line. Mutation.addOrUpdateCartPayment with payment.paymentGatewayCode "LoyaltyPaymentMethod" returned HTTP 200 with data.addOrUpdateCartPayment null and errors[0] = {message "Error trying to resolve field 'addOrUpdateCartPayment'.", path [addOrUpdateCartPayment], extensions.code VALIDATION} on 3 of 3 attempts. The response gives no reason text. With "NoSuchGateway" the error was identical; with DefaultManualPaymentMethod the call succeeded. POST /api/payment/search {storeId B2B-store} listed LoyaltyPaymentMethod with isActive false, and cart.availablePaymentMethods did not include it. So xCart refuses any gateway the store does not offer, before the loyalty cart validator runs, and the payment never reaches the cart. A LOYALTY_PAYMENT_METHOD_NOT_ALLOWED check therefore needs a stand where the method is active.
