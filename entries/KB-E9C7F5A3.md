---
id: KB-E9C7F5A3
subject: Payment refunds are not limited by the captured or payment amount, and a full refund leaves the PaymentIn status Paid
plane: experiential
question: Can refunds on a payment exceed the captured amount, and does a full refund move the payment to Refunded?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.orders 3.1018.0
  - axis: surface
    value: platform-api
anchors:
  - coordinate: POST /api/order/payments/payment/refund
  - coordinate: POST /api/order/payments/payment/capture
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T09:30:15.285Z
    by: session:624d35a1
    who: kutasinaelena
---
Orders 3.1018.0, DefaultManualPaymentMethod, admin token. Payment sum 100, captured 80 via POST /api/order/payments/payment/capture, refunded 15, then POST /api/order/payments/payment/refund with amount 200 -> 200 {succeeded:true, refundStatus:Processed}; payment now carries refunds 15 + 200 against a capture of 80 and its status stays Paid. PaymentFlowService only rejects Amount null or <= 0; the file is byte-identical at tags 3.1009.0 and 3.1018.0, so the behaviour predates Stable 16. A full refund equal to the payment amount also leaves status Paid (it never becomes Refunded), so a fully refunded payment can be refunded again. The user guide states only that refunding is possible for Paid payments and gives no amount limit. Capture/refund requests without transactionId fail with PAYMENT_FAILED 'An error occurred while saving the entity changes' (NOT NULL TransactionId).
