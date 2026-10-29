---
id: KB-A5FA3A12
subject: On single-step /cart, GA4 add_payment_info fires only once payment method AND billing address are both set; selecting the payment method with no billing address sends nothing
plane: experiential
question: When does GA4 add_payment_info fire on the storefront cart page?
questions:
  - text: Why is there no add_payment_info after picking a payment method on /cart?
  - text: Does add_payment_info need a billing address?
status: active
appliesTo:
  - axis: event
    value: add_payment_info
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: window.dataLayer
  - coordinate: Mutation.addOrUpdateCartPayment
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    at: 2026-10-29T00:26:34.295Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
    note: "distinct: About add_payment_info trigger timing (payment + billing validity), not add_to_cart source_route nor add_shipping_info ordering."
---
Signed-in user, /cart, shipping Fixed Rate (Ground). Billing 'Same as shipping address' was unchecked (it unchecks after toggling Pickup then Shipping). Selecting Bank card (Datatrans), then Manual: AddOrUpdateCartPayment mutation sent each time, but no add_payment_info in window.dataLayer. Re-selecting Datatrans and ticking 'Same as shipping address' immediately pushed add_payment_info {payment_type: DatatransPaymentMethod, currency USD, value 199.2 (= order Total incl. shipping+tax), items_count 1}, and Place order became enabled. So the event follows payment-data validity, not the method selection.
