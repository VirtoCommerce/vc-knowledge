---
id: KB-16AEF0C2
subject: order operation numbering and parentage
plane: experiential
question: is a shipment number the order's numbering scheme with a different prefix?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: OrderShipmentType.number
  - coordinate: OrderShipmentType.parentOperationId
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:50:32.066Z
---

Same scheme, independent counter. Every operation on an order is numbered PREFIX + yymmdd + dash + a five-digit sequence -- CO for the CustomerOrder, SH for the Shipment, PI for the PaymentIn -- and the sequence is per prefix, not per order, so one checkout on a quiet day yields CO260914-00001, SH260914-00001 and PI260914-00001 and the matching numbers are a coincidence of all three counters being at the same point. You therefore CANNOT infer an order number from a shipment number or vice versa: on a busy day the fifth order's shipment may be SH<date>-00012. Link them by the real key instead -- the shipment carries customerOrderId, and the order carries childrenOperations listing both the PaymentIn and the Shipment. Note that shipment.parentOperationId is null even though the Admin blade draws the shipment as a child node under the order in its operations tree; the tree is built from the order's childrenOperations, not from the child's back-pointer, so an agent that walks parentOperationId upward finds nothing.
