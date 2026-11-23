---
id: KB-BCAD6E81
subject: A non-rep buyer opening /company/my-customers/{own orgId} is redirected to /account/dashboard; no Sales Rep Hub section and no salesRep* GraphQL operation is sent
plane: experiential
question: What happens when a buyer without sales-rep access opens the Sales Rep Hub customer profile URL of their own organization?
questions:
  - text: Does a non-rep buyer see the Sales statistics block on /company/my-customers/{orgId}?
  - text: Is SalesRepMonthlyOrderStatistics sent for a buyer without sales-rep:access?
status: active
appliesTo:
  - axis: role
    value: buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: Query.salesRepCustomerOrderStatistics
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 36b0ead443a99f6c
    conditions: module:VirtoCommerce.SalesRep=3.1026.0-pr-38-5280; theme=2.63.0-pr-2635-8318; store=B2B-store; setting:SalesRep.Enabled=true
    at: 2026-11-23T22:32:49.511Z
    by: session:2d819cd0
    who: kutasinaelena
    note: "distinct: KB-E1585C7A is about the route parameter being a GUID; this records the access gate for a non-rep buyer on that route (redirect, no salesRep* request)."
---
Signed in as a plain buyer (member of an organization that a sales rep serves, no sales-rep:access), typed /company/my-customers/<that org GUID> into the address bar. Within ~5 s the app landed on /account/dashboard; the account rail has no Sales Rep Hub section (only the buyer-facing Sales reps link). The GraphQL calls on that navigation were the buyer set (GetLayout scope accountDashboard, GetOrderStatistics, GetMonthlyOrderStatistics, GetOrderStatisticsHistory, GetOrders, menus, cart...) — no SalesRepCustomer, SalesRepMonthlyOrderStatistics or any salesRep* operation, and no GetLayout with scope salesRepCustomerProfile.
