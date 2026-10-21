---
id: KB-50AFCCD6
subject: Without order:read_prices, GET order zeroes inPayments[].captures/refunds amounts but childrenOperations still carries capture/refund amounts with withPrices true, and the Admin operations tree and capture blade display them
plane: experiential
question: Does a user without order:read_prices see capture or refund amounts for an order anywhere?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.orders 3.1018.0
  - axis: surface
    value: admin-spa
  - axis: surface
    value: platform-api
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: /workspace/orders
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T23:18:35.752Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T09:30:15.180Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-observed 2026-10-15 via REST with a fresh role [order:access, order:read, platform:access]: inPayments[0].captures/refunds amount 0 withPrices false, while childrenOperations[PaymentIn].childrenOperations Capture amount 80 / Refund amount 15 carry withPrices true."
  - method: observation
    deployment: vcptcore_regression
    build: 5609fc0c36e7302c
    conditions: platform=3.1076.7; module:VirtoCommerce.Orders=3.1018.0; module:VirtoCommerce.XOrder=3.1014.0; role=order:access+order:read+platform:access (no order:read_prices)
    at: 2026-10-21T15:15:31.944Z
    by: session:624d35a1
    bodyHash: 05aac4f3d931
    defect: suspected
    who: kutasinaelena
    note: "Re-observed 2026-10-21 on the same Orders 3.1018.0 build with a fresh role and an order whose capture (80) and refund (15) were created through POST /api/order/payments/payment/capture and /payment/refund. GET /api/order/customerOrders/{id}: inPayments[0].captures[0].amount=0 and refunds[0].amount=0 with withPrices=false, while childrenOperations[PaymentIn].childrenOperations carry Refund amount 15 and Capture amount 80 with withPrices=true, under the same ids as the masked copies. Admin SPA (logged in as that user): the operations tree shows 15.00 USD / 80.00 USD and the Capture blade shows Amount 80.00, while the order Discount field is masked. Source reading at vc-module-order 3.1018.0: PaymentIn.Clone re-clones Captures/Refunds but never re-runs FillChildOperations, so the cloned payment's ChildrenOperations still point to the un-cloned, un-masked capture/refund objects."
---
On Orders 3.1018.0 (price protection VCST-3912 present), a role holding every order permission except order:read_prices reads an order with a 250 capture and 20 refund. GET /api/order/customerOrders/{id}: order/shipment/payment totals are 0 and inPayments[0].captures[0].amount / refunds[0].amount are 0 with withPrices false, BUT childrenOperations[PaymentIn].childrenOperations[Capture|Refund] carry amount 250 / 20 with withPrices true. The Admin SPA order blade operations tree renders 'Capture ... 250.00 USD' and 'Refund ... 20.00 USD', and the Capture blade opened from the tree shows Amount 250.00, while list grid, line items and discount widgets are masked '#.##'. Stable 15 (Orders 3.1009.0) has no withPrices protection at all, so this is specific to the new feature.
