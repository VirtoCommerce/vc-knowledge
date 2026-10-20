---
id: KB-F34CDFDB
subject: The Tasks app Assignee picker searches customer Contact and Employee members by keyword, not platform user logins
plane: experiential
question: What does the Tasks app Assignee picker search (POST /api/task-management/members/search), and does typing the admin login find the platform admin?
questions:
  - text: Can a task be assigned to the platform admin account from the Tasks app?
  - text: Why does searching 'admin' in the task Assignee field return a contact instead of the administrator?
concepts:
  - id: member-search
status: active
appliesTo:
  - axis: module
    value: task-management
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/task-management/members/search
  - coordinate: /apps/tasks
evidence:
  - method: observation
    deployment: local
    conditions: role=Administrator
    at: 2026-10-20T15:44:47.660Z
    by: session:70c763de
    who: yuskithedeveloper
---
Admin signed in, Tasks app (/apps/tasks) > All tasks > Create, Assignee field (marked required). Opening the dropdown sent POST /api/task-management/members/search with {keyword:"", take:20, skip:0, objectIds:[], memberTypes:["Contact","Employee"], deepSearch:true} and listed member display names (contacts and employees). Typing 'admin' in its 'Search options' box re-sent the request with keyword 'admin', and it returned totalCount 1: a B2B organization-maintainer contact whose name contains 'admin'. The platform administrator login 'admin' was not offered. Selecting a member stores its member id as responsibleId and its name as responsibleName on POST /api/task-management. The user guide says the Assignee is 'the employee the task is assigned to'. A test step 'search the admin email/login and select the result' therefore picks whichever contact or employee name matches on that stand, not the signed-in administrator.
