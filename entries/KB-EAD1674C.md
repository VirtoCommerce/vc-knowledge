---
id: KB-EAD1674C
subject: TaskManagement REST finish with completed=false closes a task as isActive=false, completed=false, and the owning sales rep still lists it and can delete it
plane: experiential
question: How do I cancel (close without completing) a sales-rep task through the platform API, and does the rep still see it?
status: active
appliesTo:
  - axis: module
    value: taskmanagement
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: POST /api/task-management/finish
  - coordinate: POST /api/task-management/search
  - coordinate: Query.salesRepTasks
  - coordinate: Mutation.deleteSalesRepTask
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T11:54:15.978Z
    by: session:5e2a1103
    who: Lenajava1
---
Observed 2026-10-07 with an admin token. POST /api/task-management/finish?id=<id>&completed=false with JSON body {} returns 200 and the WorkTask; a GET then reads isActive:false, completed:false, status:null, dueDate unchanged. The same call with no request body returns HTTP 415 (the endpoint requires a JSON body even though the parameters are in the query). The task's owning sales rep still receives the row from Query.salesRepTasks on /graphql/sales-rep, with isActive:false and completed:false, and Mutation.deleteSalesRepTask as that rep deletes it (returns true). The rep-scoped GraphQL exposes no cancel mutation, so this REST call is the only API path to a closed-but-not-completed task. A task's responsibleId is the rep's CONTACT id, not the security user id: POST /api/task-management/search with responsibleIds=[userId] returns 0 rows, with the contact id it returns the rep's tasks.
