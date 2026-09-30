---
id: KB-AB35BCDC
subject: what an order line item stops carrying once the cart becomes an order
plane: experiential
question: why can I not read listPrice off an order line item when the cart had one
questions:
  - text: Where can I see the original list price of an item on my placed order?
  - text: Why does an order line item have no listPrice or salePrice while the cart line had them?
  - text: Which price fields does an order line item carry compared with a cart line item?
  - text: How do I recover the per-unit list price from an order line?
concepts:
  - id: order-line-item
  - id: price
  - id: cart-line-item
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: OrderLineItemType.listTotal
  - coordinate: LineItemType.listPrice
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T14:00:13.799Z
    by: session:6d4f2631
---

The cart line item and the order line item are NOT the same money shape, and the difference is silent. A cart LineItemType carries listPrice/listPriceWithTax, salePrice/salePriceWithTax, placedPrice, extendedPrice and listTotal. The order OrderLineItemType drops listPrice and salePrice entirely and adds price/priceWithTax in their place; it keeps placedPrice, extendedPrice, listTotal and discountAmount, each with a WithTax twin. So the per-unit list price a buyer was shown is not preserved on the order as a per-unit figure -- only listTotal survives, and you recover the unit figure by dividing by quantity. Verified on a placed order: for a line of 2 at 13.10 the order holds price 13.10, placedPrice 13.10, extendedPrice 26.20, listTotal 26.20, and no listPrice field at all. Both the REST record from GET /api/order/customerOrders/{id} and the GraphQL type agree on this, so it is the model and not a projection.
