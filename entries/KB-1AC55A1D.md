---
id: KB-1AC55A1D
subject: A checkout payment stamps capturedDate at the same instant as authorizedDate even when it only reached Authorized and was never captured, so capturedDate cannot be used to decide whether funds were taken
plane: experiential
question: does an order's inPayments[].capturedDate mean the payment was actually captured?
status: active
appliesTo:
  - axis: gateway
    value: cybersourcepaymentmethod
  - axis: gateway
    value: skyflowpaymentmethod
  - axis: module
    value: virtocommerce.orders@3.1015.0
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: POST /api/order/customerOrders/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:12:45.324Z
    by: session:local_e8
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:12:45.324Z
    by: session:local_e8
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T18:43:00.974Z
    by: session:d81102ae
    note: "Independently reproduced on a different order (CO260919-00024 / PI260919-00027, CyberSourcePaymentMethod, $406.80): paymentStatus/status \"Authorized\", capturedDate and authorizedDate 0.7 microseconds apart, captures[] empty, and the stored gateway transaction responseData shows status \"AUTHORIZED\" with a live \"capture\" _links href still on offer. Cross-tab over 393 payments (400 most-recent orders, vcst-qa): 22/22 of all CyberSourcePaymentMethod payments with capturedDate set are stuck at paymentStatus \"Authorized\" (none ever reach \"Paid\" in this sample) - so on this deployment capturedDate-set-for-CyberSource is 100% non-predictive of an actual capture, not just occasionally wrong. Root cause found in source: VirtoCommerce.CyberSourcePayment.Data.Providers.CyberSourcePaymentMethod.PaymentApproved() (vc-module-cyber-source) sets payment.CapturedDate = DateTime.UtcNow unconditionally in the same block that also sets result.NewPaymentStatus = SingleMessageMode ? Paid : Authorized - i.e. CapturedDate is written regardless of whether SingleMessageMode actually produced a settled/Paid result. AuthorizeNetPaymentMethod and DatatransPaymentMethod gate CapturedDate correctly (only on real Sale/capture success), so this looks CyberSource-module-specific, not a shared/core order-module bug."
---
No. On vcst-qa (VirtoCommerce.Orders 3.1015.0, VirtoCommerce.CyberSourcePayment 3.1002.0) order CO260921-00002 read via GET /api/order/customerOrders/{id} has inPayments[0] with paymentStatus "Authorized", status "Authorized", authorizedDate 2026-09-21T07:59:53.9921665Z and capturedDate 2026-09-21T07:59:53.9921674Z — 0.9 microseconds apart, i.e. written by the same code path, not by a separate capture. Three independent signals say no capture happened: captures[] is empty; refunds[] is empty; and the gateway's own stored transaction responseData reports status "AUTHORIZED" with orderInformation.amountDetails.authorizedAmount "811.20" and an _links.capture href (/pts/v2/payments/{id}/captures, POST) still on offer, which CyberSource only returns while a payment is authorized and uncaptured. So capturedDate is populated by the post-process step regardless of the outcome. The pattern is not gateway-specific: across the last 8 B2B-store orders, every payment that reached a terminal gateway state has authorizedDate == capturedDate to the same second — three SkyflowPaymentMethod payments at status "Paid" and this CyberSource one at status "Authorized" — while the four DefaultManualPaymentMethod payments at status "New" have both fields null. Consequence: to decide whether funds were actually taken, read paymentStatus/status (Authorized vs Paid/Captured), captures[], or the gateway transaction's own status — never capturedDate, which will report a capture that did not occur and will do so silently. Separately on the same record: PaymentIn.price/total/totalWithTax are all 0 while sum is 811.2, which is the known-and-correct split described in KB-0DD47BD1, not a defect.
