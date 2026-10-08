---
id: KB-1FFA684C
subject: salesRepActivities, salesRepCustomerActivitySummary and salesRepCustomerInsights return data null (no errors) for a caller without sales-rep:access, on both /graphql/sales-rep and the main /graphql, even for the caller's own organization
plane: experiential
question: What do the sales-rep activity queries return to a non-rep customer member, including for their own organization?
status: active
appliesTo:
  - axis: actor
    value: non-rep-member
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepActivities
  - coordinate: Query.salesRepCustomerActivitySummary
  - coordinate: Query.salesRepCustomerInsights
  - coordinate: POST /graphql/sales-rep
  - coordinate: POST /api/sales-rep/analytics-diagnostics
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T11:40:28.201Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
---
Password-grant tokens (storeId=B2B-store) for four non-rep B2B members (Organization employee, maintainer, manager, Purchasing agent; permissions without any sales-rep entry) called Query.salesRepActivities (no organizationId, own org id, an unrelated org id), Query.salesRepCustomerActivitySummary and Query.salesRepCustomerInsights on POST /graphql/sales-rep and on POST /graphql. Every call returned HTTP 200 with the field null and no errors[] - never rows, never an authorization error. The same three queries with a rep token returned identical data on both endpoints; the storefront itself calls them on the main /graphql endpoint. POST /api/sales-rep/analytics-diagnostics returned 403 for both member and rep tokens.
