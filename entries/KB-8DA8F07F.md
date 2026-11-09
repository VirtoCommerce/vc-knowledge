---
id: KB-8DA8F07F
subject: POST /api/task-management answers 401 (not 403) to a signed-in back-office user holding task:access + task:read but no task:create; search answers 200
plane: experiential
question: What does POST /api/task-management return for a non-admin user without task:create?
questions:
  - text: Does the Task Management create endpoint return 403 for a read-only role?
  - text: Which permissions does POST /api/task-management/search need?
  - text: How is task creation gated for a Manager user with only task:read?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.taskmanagement
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/task-management
  - coordinate: POST /api/task-management/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 0a0b8dac8ab16bcc
    conditions: platform=3.1076.8; role=task:access+task:read only; isAdministrator=false
    at: 2026-11-09T09:43:30.253Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
A Manager account (isAdministrator=false) whose only role grants task:access and task:read got a token from a bare password grant at /connect/token (200); /api/platform/security/currentuser listed exactly those two task permissions. With that token POST /api/task-management/search {take:1} returned 200, and POST /api/task-management with a minimal body returned 401 Unauthorized — not 403 Forbidden — so a case asserting 403 for the missing create permission reads a 401 on this build.
