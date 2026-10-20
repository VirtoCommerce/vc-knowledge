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
---
As the owning rep on /graphql/sales-rep: (1) a call linked to a served org, updated with name/type/dueDate/description but no customerId, comes back customerId:null, customerName:null - the link is removed. (2) After an admin removes the org from the rep (PUT /api/sales-rep), reads return customerId/customerName null and the storefront list line says "Customer not assigned"; an update without customerId (UI Edit call unchanged save, which omits customerId, and an API time change) keeps the hidden link - after the org is re-assigned the task names it again. (3) An update while hidden that passes another served org as customerId replaces the hidden link; after re-assignment the task names the new org. The same request shape therefore unlinks or keeps depending on whether the rep currently serves the linked org.
