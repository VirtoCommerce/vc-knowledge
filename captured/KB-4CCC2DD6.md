---
id: KB-4CCC2DD6
subject: what Cancel document on an order actually cancels
plane: experiential
question: I cancelled an order in Admin -- is everything on it cancelled now
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: CustomerOrder.isCancelled
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T14:03:41.285Z
    by: session:6d4f2631
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:53:03.608Z
    by: session:ea806a91
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T09:02:33.256Z
    by: session:0a2d9431
---

Cancel document on an order blade is a real cancel and a PARTIAL one. It asks for a reason in a modal and, on Confirm, sets the order's status to Cancelled, isCancelled true, cancelledDate and cancelReason, and it cascades to the in-payment, which also goes status Cancelled and isCancelled true. It does NOT cascade to the shipment, which stays status New with isCancelled false, and it does NOT cascade to the line items, which all stay isCancelled false. So a cancelled order still contains line items that individually claim not to be cancelled and a shipment that claims to be live, and anything that reasons per line rather than per order will treat them as open. Reserved stock IS released -- the inventory each line drew down returns to its pre-order figure. One trap in getting there: the button opens a Bootstrap .modal, not the app's own dialog element, and it changes nothing until Confirm is pressed, so a check of the record straight after clicking shows the order still New and looks like the action silently failed. Note also that cancelledState stays Undefined even on a cancelled order, so it is not the field to test.
