---
id: KB-521D6ECF
subject: POST /api/task-management with an empty JSON body answers 200 and persists a nameless, typeless task (priority Lowest, isActive false)
plane: experiential
question: What does POST /api/task-management return for an empty body {} — a validation error or a created task?
questions:
  - text: Does the task-management create endpoint validate required fields like name and type?
  - text: Why is there a nameless task in the Tasks grid after a negative API test?
  - text: Does POST /api/task-management return 400 for a missing name?
status: active
appliesTo:
  - axis: module
    value: taskmanagement
  - axis: role
    value: admin
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/task-management
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 0a0b8dac8ab16bcc
    conditions: platform=3.1076.8; module:VirtoCommerce.TaskManagement=3.1005.0; role=admin
    defect: suspected
    at: 2026-11-09T12:39:58.716Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
Admin token, TaskManagement 3.1005.0. POST /api/task-management with Content-Type application/json and body {} answered HTTP 200 with a WorkTask carrying a new id and number, name null, type null, priority Lowest (enum default), isActive false, responsibleId null. The record is persisted: POST /api/task-management/search lists it. Source at tag 3.1005.0: TaskManagementController.Create binds [FromBody] WorkTask and calls SaveChangesAsync with no validation; WorkTaskEntity.Name has only [StringLength(256)], and no validator exists in the module (also none at 3.1007.0). An earlier note in suite 085 recorded HTTP 500 on a 3.1002 pre-release build; on 3.1005.0 it is 200. The Admin Tasks app marks Type and Summary as required, so the API accepts what the UI refuses. Delete such a record with DELETE /api/task-management and the id as a JSON string body.
