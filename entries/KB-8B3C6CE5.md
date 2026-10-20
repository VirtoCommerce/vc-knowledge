---
id: KB-8B3C6CE5
subject: salesRepActivities returns data null with no errors for a sales rep who serves zero organizations, while salesRepCustomers returns totalCount 0 for the same caller
plane: experiential
question: What does salesRepActivities return for a sales rep with no customers?
questions:
  - text: Does the sales-rep activity feed return an empty connection or null when the rep serves nobody?
  - text: Why is salesRepActivities null for a rep with zero memberships?
concepts:
  - id: sales-rep
status: active
appliesTo:
  - axis: actor
    value: sales-rep-zero-customers
  - axis: module
    value: sales-rep
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepActivities
  - coordinate: POST /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; module:VirtoCommerce.GoogleEcommerceAnalytics=4.1005.0-pr-61-5267; role=Sales Representative
    at: 2026-10-20T18:21:43.719Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
    note: "distinct: KB-1FFA684C covers callers WITHOUT sales-rep:access; this is a permissioned rep who serves zero organizations, and contrasts it with salesRepCustomers totalCount 0."
---
Password-grant token (storeId=B2B-store) for a permissioned sales rep with zero served organizations. POST /graphql/sales-rep: salesRepActivities(storeId) -> HTTP 200, data.salesRepActivities null, errors [] (also with take:0 and with an unserved organizationId); salesRepCustomers(storeId){totalCount} -> 0. A rep serving 5 orgs gets a full connection from the same query. Source (SalesRepActivitiesQueryHandler, vc-module-sales-rep@e5bcccb): returns null when there is no user, when GetVisibleOrganizationIdsAsync yields no orgs, or when the store is not allowed — the same 'no scope' null as salesRepCustomerInsights and the statistics queries, while the list queries (salesRepCustomers, salesRepOrders, salesRepTopSellers) return an empty result. The storefront does not read the null: /company/activities and the dashboard widget show 'No customers assigned to you yet' from salesRepCustomers.totalCount === 0.
