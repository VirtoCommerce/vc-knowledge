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
  - coordinate: /cart
  - coordinate: /checkout/completed
  - coordinate: /account/orders/{orderId}
  - coordinate: Mutation.createOrderFromCart
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T10:58:36.659Z
    by: session:local_de
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:02:53.328Z
    by: session:local_e8
    note: "Re-observed end to end while placing order CO260921-00002. Shipping address, delivery method, billing address, payment method, the CyberSource inline card form, the order comment and the Place order button all live on /cart itself; no navigation occurred between adding the item and submitting. The only route change was AFTER submit: /cart -> /checkout/completed (which shows just the order number and Show order / Home page buttons), then /account/orders/{id} for the detail. So /checkout/* exists only as a post-submit terminus on this build (storefront 2.58.0-pr-2467-1f40-1f40b001)."
  - method: observation
    deployment: virtostart
    at: 2026-09-23T08:13:51.263Z
    by: session:70b7199f
    who: Dan-BV
    note: Confirmed across three orders this run. Shipping option, address, delivery method, billing address, payment method and the card form all live on /cart; there is no intermediate checkout route. The only /checkout/* path reached is /checkout/completed after Place order, and it is minimal — order number plus "Show order"/"Home page" links, with no line items and no totals. A retry of a failed payment instead goes to /account/orders/{id}/payment.
  - method: observation
    deployment: virtostart
    at: 2026-09-23T10:13:47.703Z
    by: session:cdb27d99
    who: Dan-BV
    note: Re-confirmed across three more orders on virtostart this run (CO26092300004 Shipping/CyberSource, CO26092300005 Pickup/CyberSource, CO26092300006 Shipping/saved Skyflow card). Shipping option, address, delivery method, billing address, payment method and both card forms (CyberSource Flex Microform iframes and the Skyflow saved-card CVV iframe) all live on /cart. The only route change is /cart -> /checkout/completed after Place order, and that page carries the order number plus Show order / Home page only — no line items and no totals, so any assertion about ordered products or the confirmation total has to be made on /account/orders/{id} instead.
---
On vcst-qa (B2B-store, Ver. 2.58.0-pr-2467) the /cart page carries the whole checkout: Shipping details (Delivery option Pickup/Shipping, Shipping address, Delivery method), Payment details (Billing address with a 'Same as shipping address' checkbox checked by default, Payment method), Order comment, and the Place order button in the Order summary sidebar. There is no /checkout route to navigate to; on success the app lands on /checkout/completed showing 'Order <number> has been successfully submitted.' plus a 'Show order' link to /account/orders/<order-guid>. Place order stays disabled with 'Complete all required information to proceed.' until shipping address + delivery method + payment method are all set. With no saved address, the 'select a shipping address' control opens a 'New address' dialog directly rather than a picker; Country must be chosen from the combobox before State/Province is enabled, and State/Province only becomes required once a country is picked. Delivery methods offered: Fixed Rate (Ground) $150.00 and Fixed Rate (Air). Payment methods offered: Bank card (Authorize.Net), Bank card (CyberSource), Bank card (Datatrans), Manual, Pay with points, Bank card (Skyflow). Choosing 'Manual' reveals an extra 'Enter a purchase order number' textbox in Payment details and re-disables Place order. The placed order opens in status 'New' and still shows a 'Pay now' button, i.e. placing and paying are separate steps. Verified end to end by placing order CO260919-00028.
