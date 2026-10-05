---
id: KB-6E63AFF4
subject: an order can be deleted, not only cancelled
plane: experiential
question: Can an order be deleted outright, or is cancelling the only option?
questions:
  - text: Can a back-office user remove an order completely instead of cancelling it?
  - text: Where is the Delete control for orders in the admin orders list?
  - text: Which REST endpoint deletes customer orders by id?
  - text: Is order deletion supported by the platform, or only cancellation?
concepts:
  - id: order-deletion
  - id: customer-order
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T13:44:12.679Z
    splitFrom: KB-0C102D97
  - method: source
    module: VirtoCommerce.Orders
    version: 3.1000.4
    path: src/VirtoCommerce.OrdersModule.Data/Handlers/CancelPaymentOrderChangedEventHandler.cs
    url: https://raw.githubusercontent.com/VirtoCommerce/vc-module-order/3.1000.4/src/VirtoCommerce.OrdersModule.Data/Handlers/CancelPaymentOrderChangedEventHandler.cs
    at: 2026-09-16T07:14:24.046Z
    by: claude-opus-5
    splitFrom: KB-0C102D97
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-16T12:28:48+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round2/arm-B/report.md
    splitFrom: KB-0C102D97
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-16T12:28:48+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round2/arm-C/report.md
    splitFrom: KB-0C102D97
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:25:33.287Z
    by: session:26f59771
    note: "Seen again on B2B-store order CO260915-00001 (id 1c458e1f): order status Cancelled / isCancelled true / cancelReason set; inPayments[0] PI260915-00001 status Cancelled, isCancelled true, cancelledState Completed; shipments[0] SH260915-00001 status New, isCancelled false, modifiedDate still equal to createdDate (2026-09-15T08:17:18.3098923Z). Read from GET /api/order/customerOrders/{id} on platform 3.1007.27, Orders 3.1000.4."
    splitFrom: KB-0C102D97
---
Cancelling is NOT the only option. An order CAN be deleted - DELETE /api/order/customerOrders, operation OrderModule_DeleteOrdersByIds, and the Admin orders list carries a Delete control in the toolbar and in every row menu. Confirmed on platform 3.1007.27, Orders 3.1000.4.
