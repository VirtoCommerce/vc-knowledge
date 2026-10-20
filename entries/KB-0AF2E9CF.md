---
id: KB-0AF2E9CF
subject: "Saving a new task in the Tasks app shows no success toast: the create blade turns into the task's details blade and the grid reloads"
plane: experiential
question: Does the Admin Tasks app (/apps/tasks) show a success toast or notification after Save creates a task through POST /api/task-management?
questions:
  - text: Is there a 'Saved' or 'Task created' message after creating a task in the Tasks app?
  - text: How does a tester tell that a task was created in the Tasks app?
status: active
appliesTo:
  - axis: module
    value: task-management
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/tasks
  - coordinate: POST /api/task-management
evidence:
  - method: observation
    deployment: local
    conditions: role=Administrator
    at: 2026-10-20T15:44:39.591Z
    by: session:70c763de
    who: yuskithedeveloper
---
Admin signed in, Tasks app (/apps/tasks) > All tasks > Create. Filled Type, Summary, Description, Priority, Due and Assignee, then clicked Save. The blade sent POST /api/task-management (200, the response carries number and id). The 'Create new task' blade turned into the details blade titled '# <number>: <summary>' with a Complete / Reject / Reset / Save / Delete toolbar and a 'To Do' status chip, the 'Has unsaved changes' banner went away, and the All tasks grid reloaded (POST /api/task-management/search) with the new row on top. No toast was rendered: the accessibility snapshot taken right after the click and a viewport screenshot show none, and the header Notifications panel read 'No notifications yet'. In the deployed app bundle the details-blade Save handler only awaits the save, resets the change baseline and emits reload / onItemClick, with no notification call. The user guide (Use Tasks Module > Create tasks) mentions only an email notification to the assignee. A case asserting an in-app success toast after creating a task does not match this build.
