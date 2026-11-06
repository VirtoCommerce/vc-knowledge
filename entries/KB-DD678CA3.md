---
id: KB-DD678CA3
subject: The storefront order payment page opens for a Cancelled order and the platform accepts addOrUpdateOrderPayment on it, after which the page shows Pay now
plane: experiential
question: Can a Cancelled order's /account/orders/{id}/payment page be used to change its payment method, and does the xAPI accept it?
status: active
appliesTo:
  - axis: order-status
    value: cancelled
  - axis: role
    value: org-member
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders/{id}/payment
  - coordinate: Mutation.addOrUpdateOrderPayment
evidence:
  - method: observation
    deployment: vcst_qa
    build: 71f87f0ce75c3443
    conditions: platform=3.1094.0; theme=2.62.0-pr-2620-b47e-b47e1167; module:VirtoCommerce.XOrder=3.1017.0
    defect: suspected
    at: 2026-11-06T18:39:02.170Z
    by: session:90123997
    who: Lenajava1
    note: "distinct: KB-1FF0D0F2 covers a Manual-method order in a payable state; this is the Cancelled-order state, where the page and the mutation should have refused but accepted."
---
On vcst_qa (platform 3.1094.0, theme 2.62.0-pr-2620-b47e, XOrder 3.1017.0 per the deploy manifest), as an org member I cancelled my own unpaid test order in Admin (Cancel document). The order page /account/orders/{id} then showed Status Cancelled and no Pay now. Opening /account/orders/{id}/payment by URL still rendered Edit on Billing address and on Payment method, and the Select payment method modal had enabled Select buttons. Choosing Datatrans sent mutation AddOrUpdateOrderPayment (command.orderId, payment.id, payment.paymentGatewayCode) and got HTTP 200 with data.addOrUpdateOrderPayment.id and no errors. After reload the page showed Bank card (Datatrans) and an enabled Pay now button on the Cancelled order. The same mutation on the same order before cancelling also returned 200. Pay now was not clicked, so whether the platform refuses the payment itself is not known. The storefront source has no status check on order-payment.vue (only isApproved redirects) while order-details.vue shows Pay now only for New / Payment required; the xOrder handler source does not read the order status.
