---
id: KB-97BDC6A5
subject: On Orders 3.1009.0, a user without order:read_prices gets order total and payment sum zeroed but capture and refund amounts unmasked in both inPayments[].captures/refunds and childrenOperations, and there is no withPrices flag
plane: experiential
question: On Orders 3.1009.0, does a back-office user without order:read_prices see capture and refund amounts in GET /api/order/customerOrders/{id}?
questions:
  - text: Was capture/refund amount masking already present before the Orders 3.1018 price protection?
  - text: Which order amounts does Orders 3.1009.0 hide from a role without order:read_prices?
  - text: Is the capture amount exposure to users without read_prices new in Orders 3.1018?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.orders 3.1009.0
  - axis: surface
    value: platform-api
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.15; module:VirtoCommerce.Orders=3.1009.0; role=order:access+order:read+platform:access (no order:read_prices)
    defect: suspected
    at: 2026-10-21T15:16:04.196Z
    by: session:624d35a1
    who: kutasinaelena
    note: "distinct: Different module version (Orders 3.1009.0, before the withPrices protection): here capture/refund amounts are unmasked in BOTH inPayments and childrenOperations, so the exposure predates 3.1018; KB-50AFCCD6 is the 3.1018 partial-mask behaviour."
---
Observed 2026-10-21 on vcptcore_stable (platform 3.1039.15, VirtoCommerce.Orders 3.1009.0). A fresh role with exactly order:access, order:read and platform:access (no order:read_prices) read an order whose capture (80) and refund (15) were created through POST /api/order/payments/payment/capture and /payment/refund. GET /api/order/customerOrders/{id} returned total 0 and inPayments[0].sum 0, but inPayments[0].captures[0].amount 80 and refunds[0].amount 15, and childrenOperations[PaymentIn].childrenOperations also carried Capture 80 / Refund 15. The response has no withPrices field at all. So on this older line capture and refund amounts are not hidden from a no-read_prices user by any path; Orders 3.1018.0 masks them in inPayments but still exposes them in childrenOperations (see KB-50AFCCD6). The exposure itself predates 3.1018; what 3.1018 adds is a partial mask.
