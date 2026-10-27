---
id: KB-EA1CB4BF
subject: updateSalesRepTask without customerId unlinks a served customer but keeps a link to an organization the rep no longer serves; a served customerId replaces even a hidden link
plane: experiential
question: Does Mutation.updateSalesRepTask sent without customerId remove the task's customer link?
questions:
  - text: What happens to a sales-rep call's customer when the rep edits it while no longer serving that customer?
  - text: Is omitting customerId on updateSalesRepTask a breaking change for API clients?
  - text: Does re-assigning an organization to a rep bring back the customer name on an existing call?
status: active
appliesTo:
  - axis: module
    value: salesrep
  - axis: role
    value: sales-rep
  - axis: surface
    value: graphql-xapi
anchors:
  - coordinate: Mutation.updateSalesRepTask
  - coordinate: PUT /api/sales-rep
evidence:
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; module:VirtoCommerce.TaskManagement=3.1005.0; theme=2.60.0
    at: 2026-10-20T14:29:17.137Z
    by: session:70c763de
    who: yuskithedeveloper
  - method: observation
    deployment: vcptcore_qa
    build: 956cfff4cb0a606f
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; module:VirtoCommerce.TaskManagement=3.1005.0; store=B2B-store
    at: 2026-10-27T10:46:30.608Z
    by: session:e75fe8a4
    bodyHash: c58661c1d26a
    who: Aleksandra-Mitricheva
    note: "Part (1) re-observed on a deployed stand on both /graphql and /graphql/sales-rep: a call linked to a still-served org, updated with id/name/type/priority/dueDate and the customerId key absent, returned and read back customerId:null, customerName:null; an explicit customerId:null gives the identical result. Source at 92d48e1 (SalesRepTaskCommandHandlerBase.ApplyCustomerAsync) treats absent and null the same; only an unserved stored ObjectId is kept."
---
As the owning rep on /graphql/sales-rep: (1) a call linked to a served org, updated with name/type/dueDate/description but no customerId, comes back customerId:null, customerName:null - the link is removed. (2) After an admin removes the org from the rep (PUT /api/sales-rep), reads return customerId/customerName null and the storefront list line says "Customer not assigned"; an update without customerId (UI Edit call unchanged save, which omits customerId, and an API time change) keeps the hidden link - after the org is re-assigned the task names it again. (3) An update while hidden that passes another served org as customerId replaces the hidden link; after re-assignment the task names the new org. The same request shape therefore unlinks or keeps depending on whether the rep currently serves the linked org.
