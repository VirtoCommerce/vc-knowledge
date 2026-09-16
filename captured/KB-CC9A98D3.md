---
id: KB-CC9A98D3
subject: order address copies per operation
plane: experiential
question: does a shipment share the order's shipping address or hold its own copy?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: OrderShipmentType.deliveryAddress
  - coordinate: CustomerOrderType.addresses
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:49:48.415Z
    by: session:ea806a91
---

Its own copy, and so does every other operation. One checkout against a single saved member address produces FOUR independent OrderAddress rows, each with its own key: order.addresses[Shipping], order.addresses[Billing], shipment.deliveryAddress, and payment.billingAddress. Nothing links them back to the member address they were copied from and nothing links them to each other -- the member address keeps its own separate id in the member module. They are snapshots taken at placement, so they can and will disagree: the Admin blade for the shipment's deliveryAddress is a full editable form with its own Delete button, and editing it changes the shipment alone while the order's own Shipping address row keeps the old value. Consequences: reading order.addresses does not tell you where a shipment is going, an address correction has to be applied to each operation separately, and a later change to the member's address book never propagates to a placed order.
