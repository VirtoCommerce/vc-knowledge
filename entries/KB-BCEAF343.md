---
id: KB-BCEAF343
subject: Opening /company/my-customers/<id> of an organization the rep does not serve shows "Customer not found or not in your customers." but the profile widgets still send their queries for that id (SalesRepMonthlyOrderStatistics, SalesRepOrders, SalesRepTopSellers…); the statistics answer is null
plane: experiential
question: What does the sales-rep customer profile request and show for an organization the rep does not serve?
questions:
  - text: Does the customer profile fire SalesRepMonthlyOrderStatistics for an unserved organization?
  - text: Is the Sales statistics block rendered for a customer the rep does not serve?
concepts:
  - id: sales-rep
  - id: organization
status: active
appliesTo:
  - axis: role
    value: sales-representative
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: Query.salesRepCustomerOrderStatistics
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 36b0ead443a99f6c
    conditions: module:VirtoCommerce.SalesRep=3.1026.0-pr-38-5280; theme=2.63.0-pr-2635-8318; store=B2B-store
    at: 2026-11-23T22:25:39.933Z
    by: session:2d819cd0
    who: kutasinaelena
    note: "distinct: KB-54502E1C is the Reporting page's ?customer= fallback; this is the customer PROFILE route: what it renders for an unserved org and which widget queries it still sends."
---
As a sales rep, a full load of /company/my-customers/<GUID of an existing organization the rep does not serve> rendered only the breadcrumb, the text "Customer not found or not in your customers." and a "Back to my customers" link — no Sales statistics block and no other widget. The network log of that load still held SalesRepMonthlyOrderStatistics, SalesRepOrderFilterRules, SalesRepOrders, SalesRepTopSellerFilterRules and SalesRepTopSellers, each with organizationId = the unserved id; the statistics response was {"data":{"salesRepCustomerOrderStatistics":null}}. So the server guard holds; the client fires the widget queries before or regardless of the customer guard.
