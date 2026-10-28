---
id: KB-9E0FCF52
subject: An applied cart coupon stays on the cart after its items are removed and the cart is cleared; the next item added is discounted by it
plane: experiential
question: Does a coupon applied on /cart survive emptying the cart, so a later item added gets the discount automatically?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.addCoupon
  - coordinate: Mutation.clearCart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; role=personal customer
    at: 2026-10-28T10:08:39.772Z
    by: session:7e8ea7fb
    who: Lenajava1
---
Signed-in personal user, B2B-store, USD cart. Applied QA10OFF through the Custom code card (Coupon QA10OFF applied. Discount $2.50), then emptied the cart (the cart was cleared from the EUR view, which also empties the USD view). Later added ALCE0128 x1 to the empty USD cart and opened /cart: the Custom code card still showed QA10OFF as applied and the Discount row read -$6.00 (10 percent of 59.99) without any coupon action in that session. The coupon had to be removed explicitly with the card's Remove coupon button (message Coupon QA10OFF removed. Discount $0.00). So a test that needs a coupon-free cart must remove the coupon, not only the items.
