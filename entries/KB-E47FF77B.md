---
id: KB-E47FF77B
subject: Choosing "Cancelled" in the Admin Shell order Status dropdown and saving sets Status=Cancelled but leaves IsCancelled=false; only "Cancel document" sets the flag
plane: experiential
question: Does setting an order's status to Cancelled in the admin order blade also set IsCancelled?
questions:
  - text: Can an order end up with status Cancelled but isCancelled false through the admin UI?
  - text: Why does a Cancelled-status order still count in sales rep statistics?
  - text: What is the difference between the order Status dropdown Cancelled and the Cancel document action?
status: active
appliesTo:
  - axis: entity
    value: customer-order
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: PUT /api/order/customerOrders
  - coordinate: /apps/admin-shell#/orders/order
  - coordinate: Query.salesRepCustomerOrderStatistics
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 781831b51ee14c37
    conditions: platform=3.1093.0; module:VirtoCommerce.AdminShell=3.1003.0-pr-4-4aa6; setting:Order.Status=Processing,Completed,Payment required,Pending,New,Cancelled
    at: 2026-11-04T10:28:57.753Z
    by: session:d12b74d5
    who: Aleksandra-Mitricheva
---
On vcptcore, in the Admin Shell (/apps/admin-shell) order detail blade, the Status combobox offers the Order.Status dictionary values Processing, Completed, Payment required, Pending, New, Cancelled; a separate "Cancel document" toolbar button exists. I created a disposable order (status New, isCancelled=false), picked "Cancelled" in the Status combobox and clicked Save without using "Cancel document". GET /api/order/customerOrders/{id} then returned status=Cancelled, isCancelled=false, cancelledDate=null, cancelReason=null. The server does not derive IsCancelled from Status (no such sync in vc-module-order source). Consequence: statistics that exclude only the IsCancelled flag (salesRepCustomerOrderStatistics, BL-SR-005) keep counting an order an admin "cancelled" via the dropdown.
