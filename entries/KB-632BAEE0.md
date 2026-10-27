---
id: KB-632BAEE0
subject: createSalesRepTask called by an organization member without sales-rep:access is refused with "Access denied." (code Forbidden), even with their own organization as customerId, and writes nothing
plane: experiential
question: Can an organization member without sales-rep:access create a sales rep task (Mutation.createSalesRepTask) linked to their own organization, and what error is returned?
questions:
  - text: Can a buyer who is not a sales rep create a call on the sales rep scoped endpoint?
  - text: What does salesRepTasks return to a non-rep customer?
  - text: Is createSalesRepTask gated on sales-rep access or only on authentication?
concepts:
  - id: sales-rep
  - id: graphql-api
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.createSalesRepTask
  - coordinate: Query.salesRepTasks
evidence:
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0 (source build of vc-module-sales-rep PR 26 @ 7007756, unreleased); module:VirtoCommerce.TaskManagement=3.1005.0; store=B2B-store; role=organization member without sales-rep:access
    at: 2026-10-20T14:32:31.695Z
    by: session:70c763de
    who: yuskithedeveloper
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; module:VirtoCommerce.TaskManagement=3.1005.0; store=B2B-store; role=Purchasing agent buyer, token role __customer, permissions storefront:organization:view+storefront:user:view
    at: 2026-10-27T18:47:20.295Z
    by: session:e75fe8a4
    bodyHash: ac98c2b61458
    who: Aleksandra-Mitricheva
    note: "Same refusal on BOTH /graphql and /graphql/sales-rep: createSalesRepTask (type Call, future dueDate, customerId = buyer's own organization) -> HTTP 200, errors[0] \"Access denied.\" code Forbidden, data null. Anonymous gets \"Anonymous access denied or access token has expired or is invalid.\" code Unauthorized. Admin POST /api/task-management/search by the attempted names returned totalCount 0: nothing written."
---
A storefront buyer contact created in an organization with a Customer account (password grant with storeId B2B-store; token role __customer, no permissions, organization claim = that organization) called createSalesRepTask on /graphql/sales-rep with type "Call", a dueDate and customerId = their own organization. Response: HTTP 200, errors[0].message "Access denied.", extensions.code "Forbidden", data.createSalesRepTask null. The refusal comes before any customer check (it is not the "Customer not found." a rep gets for an unserved organization). The same token's salesRepTasks returned totalCount 0 before and after, the admin POST /api/task-management/search by the member's contact id returned 0 before and after, and the WorkTask table held no row for the attempted name.
