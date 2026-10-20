---
id: KB-A16A1DE3
subject: GET /api/task-management/tasks/types exists and returns the TaskManagement.TaskTypes allowed values as [{name}] in setting order; the Admin Tasks app loads its Type filter and Type select from it
plane: experiential
question: Does GET /api/task-management/tasks/types exist, what does it return, and where does the Admin Tasks app get its task type list?
questions:
  - text: Which REST route lists task types for the Tasks app?
  - text: Where does the Admin Tasks Type filter take its options from?
  - text: Is Call offered as a task type in the Admin Tasks app?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/task-management/tasks/types
evidence:
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.TaskManagement=3.1005.0; module:VirtoCommerce.SalesRep=3.1015.0 (source build of vc-module-sales-rep PR 26 @ 7007756, unreleased); setting:TaskManagement.TaskTypes=null (built-in list)
    at: 2026-10-20T14:33:37.302Z
    by: session:70c763de
    who: yuskithedeveloper
    note: "distinct: KB-881267E5 describes the setting itself read through GET /api/platform/settings; this is the TaskManagement module's own REST route GET /api/task-management/tasks/types (its existence and response shape) and the Admin Tasks app's use of it for the Type filter and Type select."
---
With an admin token, GET /api/task-management/tasks/types returned HTTP 200 with [{"name":"Registration Review"},{"name":"Order Review"},{"name":"Order Processing"},{"name":"Product Catalog Management"},{"name":"Pricing and Promotions"},{"name":"Content Management"},{"name":"Customer Support"},{"name":"Other"},{"name":"Call"}], i.e. the allowedValues of the global TaskManagement.TaskTypes setting (value null) in setting order, with "Call" contributed by the Sales Rep build under test. The Admin Tasks app (main menu More > Tasks, All tasks) calls this route when it opens; its Filters panel "Type filter" and the task blade's Type select list the same nine values, displayed alphabetically with "Other" last.
