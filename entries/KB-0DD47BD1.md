---
id: KB-0DD47BD1
subject: the amount a payment is for is not the payment's total
plane: experiential
question: does the admin order screen show the same totals the storefront charged
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: PaymentIn.sum
  - coordinate: CustomerOrder.paymentSubTotal
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T14:04:26.423Z
    by: session:6d4f2631
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:49:38.328Z
    by: session:ea806a91
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:25:33.143Z
    by: session:26f59771
    note: "Same split seen on CO260915-00001: PaymentIn.sum 1371.02 while PaymentIn.total, totalWithTax, price, priceWithTax are all 0.00 and the order header paymentTotal/paymentSubTotal are 0.00. The Admin operations tree renders the PaymentIn as 1,371.02 while the totals widget on the same blade reads Payment subtotal 0.00."
---

For the goods they do, to the cent: on a placed order the storefront's checkout summary, the Admin order blade, the Admin line-items blade and GET /api/order/customerOrders/{id} all carry the same subTotal, discountTotal, taxTotal and total, and the line items sum to the header. The number that does NOT line up is the payment, and it is a field-naming trap rather than a bug. The amount a payment is FOR lives in PaymentIn.sum; PaymentIn.total, totalWithTax, price and priceWithTax are the payment METHOD's own fee and tax and are 0 for a manual method. The order header's paymentTotal and paymentSubTotal roll up those fees, not the amount due. So on an order of 172.49 the Admin blade's operations tree renders the PaymentIn as 172.49 (reading sum) while the totals widget on the same blade renders Payment subtotal 0.00 (reading paymentSubTotal), both correct and both about different things. Reconcile a payment against sum; paymentTotal is not the amount owed and reading it as such makes every manual-payment order look unpaid-for.
