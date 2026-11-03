---
id: KB-AB42AC8E
subject: On the sales-rep customer profile the Orders placed - YTD card repeats the My customers row's YTD count and total, and Avg order value is that total divided by that count
plane: experiential
question: Do the customer profile statistics cards agree with the My customers row for the same customer, and how is Avg order value derived?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: /company/my-customers
  - coordinate: /company/my-customers/{id}/orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; theme=2.62.0-pr-2596-e749-e749b47f; browser=chrome; role=Sales Representative
    at: 2026-11-03T18:58:32.169Z
    by: session:c3b82fae
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; theme=2.62.0-pr-2596-e749-e749b47f; browser=firefox; role=Sales Representative; culture=en-US
    at: 2026-11-03T19:09:21.788Z
    by: session:c3b82fae
    bodyHash: f58955fb27df
    who: Lenajava1
    note: "Re-observed: row $875.00 / 7 orders (salesRepCustomers.items.ytd = orderStatistics) equals the profile Orders placed - YTD 7 / $875.00 (salesRepCustomerOrderStatistics.ytd); Avg order value $125.00 is the server-computed ytd.average field, equal to 875/7. All orders href and the landed path are the profile path + /orders, H1 '<org name> orders'. No errors[] in salesRepCustomers, salesRepCustomerOrderStatistics or salesRepCustomerOrders responses."
---
Signed in as a sales rep serving five organizations, the My customers row of one customer read $875.00 and 7 orders under YTD purchases. Its profile (/company/my-customers/{id}) showed Orders placed - YTD 7 with $875.00 and Avg order value $125.00 captioned Average per order (YTD), i.e. 875 / 7. The same profile's other cards were Orders in New status 1 ($131.00 total, of 2 created in the last 7 days), Active carts 0 items, Purchased - MTD $0.00 (0% of YTD). The All orders page of the same customer (/company/my-customers/{id}/orders) listed more orders than 7 (ten or more rows dated Oct 29, 2026 with status New), so the YTD count is the rep's own figure and must not be compared with the All orders row count. The profile mainLeft widgets rendered in the order My recent orders, Top sellers, Available coupons.
