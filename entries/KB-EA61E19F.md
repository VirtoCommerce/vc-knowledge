---
id: KB-EA61E19F
subject: On single-step /cart, add_shipping_info fires before view_cart and begin_checkout when the cart already holds a shipment, is re-sent once on the first quantity change, and an emptied cart fires the whole funnel with 0 items
plane: experiential
question: In what order do GA4 add_shipping_info, view_cart and begin_checkout fire on /cart single-step checkout, and do they repeat on quantity change or fire for an empty cart?
status: active
appliesTo:
  - axis: checkout
    value: single-step
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.changeCartItemsQuantity
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; browser=firefox; role=anonymous guest; setting:checkout_multistep_enabled=false
    at: 2026-10-13T17:24:02.797Z
    by: session:aa76d86f
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 594519586290ed24
    conditions: theme=2.61.0-pr-2570-2fc9-2fc9261a; store=B2B-store; role=signed-in B2B organization user; browser=chromium
    at: 2026-10-22T13:13:57.778Z
    by: session:b1c1bce0
    bodyHash: 5c5f3a48c4cc
    who: Lenajava1
    note: "Also holds for a signed-in org user whose cart is empty and has no shipment: loading /cart pushed view_cart {currency USD, value 0, items_count 0} then begin_checkout {value 0, items_count 0} (no add_shipping_info), on vcst_qa and on theme 2.61.0-pr-2598 against the same backend. The storefront GA events doc names Proceed to checkout as the begin_checkout trigger."
---
Guest, single-step checkout. Cart with 1 line and a saved shipping address + Fixed Rate (Air): every /cart load pushed add_shipping_info (shipping_tier Air, value 250 = the shipping cost, not the subtotal) BEFORE view_cart and begin_checkout (3/3; GA /g/collect hits in the same order). The first quantity change after each load pushed add_shipping_info again (same tier/value) plus update_cart_item; a second change pushed update_cart_item only (2/2). After removing the last line, every /cart load of that empty cart pushed add_shipping_info (0 items, value 0), view_cart (0 items) and begin_checkout (0 items) (3/3). Controls: a brand-new guest with no cart pushes no commerce event; a cart without address/method pushes view_cart then begin_checkout, and a qty change pushes update_cart_item only. Selecting a delivery method pushes one add_shipping_info. Source: cart.vue immediate watcher on isValidShipment/shipmentMethodOption fires on forceFetch, before viewCart and initialize()->beginCheckout; the cart.id reset watcher clears the dedupe marker on first load; no items.length guard.
