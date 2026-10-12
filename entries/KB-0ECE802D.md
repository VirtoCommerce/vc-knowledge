---
id: KB-0ECE802D
subject: Every order email (created, cancelled, status/payment/shipment changed, paid, sent) goes To the first non-empty order address email; the customer's contact/account email is only the fallback
plane: experiential
question: Which address do order emails such as OrderCreateEmailNotification go To — the order address email or the customer's contact/account email?
status: active
appliesTo:
  - axis: module
    value: order
  - axis: surface
    value: notifications
anchors:
  - coordinate: POST /api/order/customerOrders
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation + source (vc-module-order SendNotificationsOrderChangedEventHandler)
    deployment: vcst_qa
    at: 2026-10-12T11:13:32.392Z
    by: session:573e5582
    who: Lenajava1
---
On vcst_qa, an order created through POST /api/order/customerOrders whose two addresses carried mailbox X, for a customer whose account and contact carried a different mailbox Y, produced one OrderCreateEmailNotification in POST /api/notifications/journal To X, not Y. vc-module-order SendNotificationsOrderChangedEventHandler.SetNotificationParametersAsync sets To = GetOrderRecipientEmailAsync(order) = the first non-empty order.Addresses[].Email, falling back to the customer member's user email or first contact email only when no address has one. The same To is used for CancelOrder, NewOrderStatus, PaymentStatusChanged, ShipmentStatusChanged, OrderPaid and OrderSent email notifications. Consequence for fixtures: an order body cloned from a template whose address email is a real person's mails that person even when the order belongs to a test account. Force the address emails to a test mailbox before the POST.
