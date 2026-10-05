---
id: KB-FF7E4D5B
subject: cart-subtotal-percentage-reward-on-customerorder
plane: experiential
question: when a percentage promotion discounts the whole cart, where does that discount end up on the resulting CustomerOrder
questions:
  - text: Where does a whole-basket percentage discount appear on my placed order?
  - text: Is a cart subtotal percentage reward spread onto line items or shipments on the resulting order?
  - text: Why is subTotalDiscount zero on an order that received a percentage-off-subtotal promotion?
  - text: Which order fields carry the reduction from an order-wide percentage promotion?
concepts:
  - id: promotion-reward
  - id: discount
  - id: customer-order
status: superseded
supersededBy: KB-F1542157
appliesTo:
  - axis: principal
    value: customer
  - axis: reward
    value: percentage-off-cart-subtotal
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: CustomerOrder.discounts
  - coordinate: CustomerOrder.subTotalDiscount
  - coordinate: OrderLineItemType.discountTotal
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-10T19:56:22.148Z
    by: session:82e15c99
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T08:41:12.175Z
    by: session:0a2d9431
---
A promotion whose reward is a percentage off the cart subtotal is recorded on the resulting CustomerOrder as exactly one entry in the order-level discounts collection, and nowhere else: every line item keeps its full placedPrice with an empty discounts array and discountAmount/discountTotal of 0, the shipment discountAmount is 0, and subTotalDiscount is 0.00 as well - only the order-level discountAmount/discountTotal carry the reduction.
