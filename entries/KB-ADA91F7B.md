---
id: KB-ADA91F7B
subject: cancelling an order cancels its payment on a background job but never its shipment
plane: experiential
question: What happens to an order's shipment and payment when the order is cancelled?
questions:
  - text: If I cancel my order, is the delivery also called off?
  - text: Why do cancelled orders still show open shipments in the fulfilment queue?
  - text: How long after an order is cancelled does its payment become cancelled, and by what mechanism?
  - text: Which order change handler cascades cancellation, and why does it skip shipments?
concepts:
  - id: order-cancellation
  - id: shipment
  - id: order-payment
status: active
appliesTo:
  - axis: surface
    value: rest
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
Cancelling an order cascades to the PAYMENT and not to the SHIPMENT. The order goes to Cancelled with isCancelled true, cancelledDate set and cancelReason carrying the dialog text; its inPayment follows about 1.7 seconds later on a background job, reaching status Cancelled, isCancelled true and cancelledState Completed; its shipment stays at status New with isCancelled false and a modifiedDate still equal to its createdDate, never written again. The mechanism is that CancelPaymentOrderChangedEventHandler collects changedEntry.NewEntry.InPayments only, and no shipment-cancelling handler exists anywhere in the Orders module. So a cancelled order leaves a live shipment document behind, and anything counting open shipments - a fulfilment queue, a warehouse worklist - keeps seeing it; this is why a store used for order testing accumulates New shipments belonging to orders that are all Cancelled. Reproduced independently by three separate runs on 2026-09-15.
