---
id: KB-1FF0D0F2
subject: A storefront order placed with the Manual payment method lands on /account/orders/{id}/payment with a note that the method does not support online payment; switching it to Authorize.Net through the page's Edit modal shows the card form and a Pay now button
plane: experiential
question: How do I get an order to a state where /account/orders/{id}/payment shows Edit buttons and a Pay now button?
status: active
appliesTo:
  - axis: role
    value: buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders/{id}/payment
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    at: 2026-10-27T19:38:59.530Z
    by: session:e0e42495
    who: Lenajava1
---
On vcst_qa (storefront 2.61.0-pr-2577-4c17), as a B2B buyer I placed an order from /cart (checkout lives on /cart) choosing Fixed Rate (Ground) and Manual; Place order stays disabled until the purchase order number field that appears under Manual is filled? Not verified: it completed after I selected Manual and the click went through. The order opened at /account/orders/{id}/payment shows Billing address and Payment method blocks, each with an Edit button at md+, and the text 'The payment method you selected does not support online payment' with no Pay now button. Edit on Payment method opens a 'Select payment method' dialog; selecting Bank card (Authorize.Net) and OK renders a card form and a Pay now button; paying with the Authorize.Net sandbox card leads to a Payment successful page (status Processing) with Back to order, Orders list and Continue shopping buttons.
