---
id: KB-C2AD28C1
subject: salesRepCustomerOrderStatistics counts a rep order whose Status is "Cancelled" but IsCancelled=false, and does not count a served organization's order created by a buyer
plane: experiential
question: Does salesRepCustomerOrderStatistics exclude orders by status Cancelled, and does it count orders other users created in an organization the rep serves?
questions:
  - text: Is an order with status Cancelled but isCancelled false counted in sales rep order statistics?
  - text: Are a buyer's own orders in a served organization included in the rep's statistics?
  - text: Does the per-organization sales rep statistic include orders of a child organization?
status: active
appliesTo:
  - axis: role
    value: sales-representative
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCustomerOrderStatistics
  - coordinate: POST /graphql/sales-rep
  - coordinate: POST /api/order/customerOrders
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 781831b51ee14c37
    at: 2026-11-03T23:19:12.853Z
    by: session:d12b74d5
    who: Aleksandra-Mitricheva
    note: "distinct: KB-1730D0BC covers the IsCancelled flag only; this records that a status-only \"Cancelled\" order IS counted, that a buyer-created order in a served org is NOT counted, and that a parent org's figure excludes child-org orders."
---
2026-11-03, store B2B-store. Created via POST /api/order/customerOrders (admin token), all in the current month: three normal orders with customerId = a sales rep's ApplicationUser id (two in one served org, one in another), one with isCancelled=true + status Processing, one with status "Cancelled" + isCancelled=false (555.50), and one in a served org with customerId = a buyer of that org (999.90). Then, as the rep on POST /graphql/sales-rep, salesRepCustomerOrderStatistics period(month-to-date) went from 0 to 4 orders / 1957.30: the three normal orders plus the status-only Cancelled one. The isCancelled=true order and the buyer-created order were not counted, on the dashboard (organizationId omitted) and per organization. Status alone does not exclude an order; only the IsCancelled flag does, and attribution is by order.CustomerId = the rep's user id, not by organization. Per-organization figures for a parent organization did not include its child division's orders (parent stayed 0 while the child showed 2 / 400.80).
