---
id: KB-6BF8D12D
subject: DELETE /api/task-management answers 204 but does not remove the task records
plane: experiential
question: Does DELETE /api/task-management actually delete tasks, or does it return 204 and leave them?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: DELETE /api/task-management
  - coordinate: POST /api/task-management/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: module:VirtoCommerce.TaskManagement=3.1005.0; role=admin
    at: 2026-11-05T14:42:47.771Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
As admin, DELETE /api/task-management with four task ids (tried both as repeated ids= query parameters and as a JSON array body) returned HTTP 204 both times, yet POST /api/task-management/search immediately afterwards still returned all four tasks (2 active, 2 inactive/archived). A test cleanup cannot rely on this call to remove tasks; assert absence in the UI only, or accept leftover AGENT-TEST tasks.
