---
id: KB-AD1FA66B
subject: where a cart's money lives, and which copy goes stale
plane: experiential
question: if a promotion changes while a shopper's cart is alive, does the cart still hold the old discount?
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-xapi
  - axis: principal
    value: customer
anchors:
  - coordinate: GET /api/carts/{id}
  - coordinate: Query.cart
  - coordinate: CartType.discounts
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T13:40:56.474Z
---

A live cart carries money in TWO places and they can disagree. READ SIDE: every figure the storefront shows is recomputed at read time from the promotions live at that instant -- create, edit or disable a promotion and the very next Query.cart reflects it with no cart mutation in between (that is @kb(KB-35A09C64), which held four times here). STORED SIDE: GET /api/carts/{id} returns whatever the last cart MUTATION wrote, and nothing else refreshes it. So read @kb(KB-35A09C64)'s phrase 'not frozen into the stored cart' as a claim about the READ PATH only -- on the stored record the opposite is true. A read never writes back: the storefront cart was read repeatedly showing subTotal 319 and a 31.90 reward while GET /api/carts/{id} kept returning subTotal 0, listPrice 0 and discounts [] on a cart not mutated since its line was added. A mutation writes back everything: after one unrelated mutation (choosing a shipment method, which changes no price) the stored cart carried subTotal, discountTotal, total and a full discount row with promotionId, name and description -- the same snapshot shape an order's discount row has. After marketing edits the promotion the two surfaces then disagree INDEFINITELY, until the shopper next touches the cart: observed with the promotion first made ineligible and later deactivated while GET /api/carts/{id} went on serving total 255.20 and the promotionId of a promotion that was by then switched off, and a storefront read of the same cart in the same seconds returned total 319.00 with discounts []. The next mutation replaces the stale row silently -- no zero-amount row, no marker, nothing saying a reward was ever there. Anything server-side that reads the stored cart (an abandoned-cart mailer, a CSR, a report) is reading the older of two true answers, and the direction of the error is whichever way the promotion moved.
