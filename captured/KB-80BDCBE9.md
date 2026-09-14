---
id: KB-80BDCBE9
subject: shipment lifecycle states and their controls
plane: experiential
question: what states can a shipment be in and what moves it between them?
status: retired
supersededBy: KB-4C5627CE
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: OrderShipmentType.status
  - coordinate: OrderShipmentType.isCancelled
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:51:04.379Z
---

Five: Cancelled, New, PickPack, ReadyToSend, Send (the value is Send, not Sent). A checkout-created shipment starts at New. What moves it is an operator, not a background process: the Admin shipment blade exposes status as a plain select listing all five, so any value can be chosen from any other with no gate, no wizard and no required precondition -- a shipment with zero items can be set to Send. Status is also NOT the whole state of a shipment: isApproved, isCancelled and cancelledState (Undefined here) are separate fields that the status select does not touch, so Cancelled-the-status and isCancelled-the-flag are two different things and can disagree. The shipment's status is likewise independent of the order's own status -- they are separate fields on separate records, and cancelling the order does not have to move the shipment, which is why a Cancelled order can sit above a shipment still reading New.

**Retired.** Its closing clause asserted that status Cancelled and the isCancelled flag can disagree on a shipment. I never observed that pair disagreeing -- I observed them AGREEING (New / false), and inferred the rest. Replaced with what was actually seen, which makes the same practical point without the unobserved claim. Superseded by KB-4C5627CE.
