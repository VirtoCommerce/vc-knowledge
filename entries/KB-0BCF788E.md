---
id: KB-0BCF788E
subject: An admin-REST WorkTask with objectType CustomerOrder and responsibleId = the rep's contact id is listed by the rep's salesRepTasks with customerId and customerName null
plane: experiential
question: Does a sales rep see a task created via POST /api/task-management that is linked to an order (objectType CustomerOrder) rather than to a customer, and what customer does it show?
questions:
  - text: How do I create a sales-rep call linked to an order instead of an organization?
  - text: What does salesRepTasks return for a task whose ObjectType is not the customer link?
status: active
appliesTo:
  - axis: module
    value: taskmanagement
  - axis: surface
    value: platform-rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /api/task-management
  - coordinate: Query.salesRepTasks
  - coordinate: Query.salesRepTaskTypes
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    at: 2026-10-27T18:48:18.416Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
    note: "distinct: KB-2A7CB639 is about a dateless task with no object link; this is about an order-linked task (objectType CustomerOrder) and how salesRepTasks renders its customer fields (null)."
---
Admin token: POST /api/task-management with name, type "Call", priority Normal, dueDate +3 days, isActive true, responsibleId = the rep's Contact (member) id, responsibleName, organizationId = one of the rep's served orgs, storeId B2B-store, objectType "CustomerOrder", objectId = an existing order id. Returned 200; GET /api/task-management/{id} reads objectType/objectId back unchanged. Signed in as that rep (password grant with storeId, no client_id/scope), Query.salesRepTasks(keyword:<name>, storeId) on /graphql/sales-rep returned the row (type Call, isActive true, completed null) with customerId null and customerName null - the order link is not surfaced as a customer. Rep-created tasks on the same stand carry objectType null. "Call" is one of GET /api/task-management/tasks/types and of Query.salesRepTaskTypes there.
