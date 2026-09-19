---
id: KB-50EBEEE9
subject: Storefront checkout happens on the /cart page itself; there is no separate checkout route until the order completes.
plane: experiential
question: what does the storefront require to place an order, and where does checkout happen
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /checkout/completed
  - coordinate: /account/orders/{orderId}
  - coordinate: Mutation.createOrderFromCart
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T10:58:36.659Z
    by: session:local_de
---
On vcst-qa (B2B-store, Ver. 2.58.0-pr-2467) the /cart page carries the whole checkout: Shipping details (Delivery option Pickup/Shipping, Shipping address, Delivery method), Payment details (Billing address with a 'Same as shipping address' checkbox checked by default, Payment method), Order comment, and the Place order button in the Order summary sidebar. There is no /checkout route to navigate to; on success the app lands on /checkout/completed showing 'Order <number> has been successfully submitted.' plus a 'Show order' link to /account/orders/<order-guid>. Place order stays disabled with 'Complete all required information to proceed.' until shipping address + delivery method + payment method are all set. With no saved address, the 'select a shipping address' control opens a 'New address' dialog directly rather than a picker; Country must be chosen from the combobox before State/Province is enabled, and State/Province only becomes required once a country is picked. Delivery methods offered: Fixed Rate (Ground) $150.00 and Fixed Rate (Air). Payment methods offered: Bank card (Authorize.Net), Bank card (CyberSource), Bank card (Datatrans), Manual, Pay with points, Bank card (Skyflow). Choosing 'Manual' reveals an extra 'Enter a purchase order number' textbox in Payment details and re-disables Place order. The placed order opens in status 'New' and still shows a 'Pay now' button, i.e. placing and paying are separate steps. Verified end to end by placing order CO260919-00028.
