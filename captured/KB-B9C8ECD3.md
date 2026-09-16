---
id: KB-B9C8ECD3
subject: the scoped discount totals on an order
plane: experiential
question: the order has a discount but subTotalDiscount is zero - where did the money go?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: CustomerOrder.subTotalDiscount
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:27:45.763Z
    by: session:26f59771
---

A cart-subtotal reward is recorded ONCE, as an order-level Discount row, and none of the scoped discount totals is written with it. CustomerOrder declares subTotalDiscount, subTotalDiscountWithTax, shippingDiscountTotal, shippingDiscountTotalWithTax, paymentDiscountTotal and paymentDiscountTotalWithTax, and on orders carrying a percentage-off-cart-subtotal promotion every one of them reads 0.0 while discountTotal carries the rounded amount, discountAmount the unrounded one, and discounts[] the single row with promotionId, name, description and money. The line items are untouched too (items[].discountAmount 0.0). So a report that sums the per-scope fields to explain a discount gets zero and concludes there is none, and the only complete answer is discountTotal plus the discounts collection. The scoped fields are presumably for rewards of those scopes - shipping and payment - which this observation does not cover: what it establishes is that a CART-SUBTOTAL reward does not land in subTotalDiscount, on two orders with two different promotions.

**Anchor corrected.** `CustomerOrder.discountTotal` → `GET /api/order/customerOrders/{id}` — REST DTO field names are not projected coordinates; the route that serves them is, and subTotalDiscount stays named in the body and in the other anchor.
