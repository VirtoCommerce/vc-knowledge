---
id: KB-EAE167D3
subject: At 375px /company/tasks renders each task as a stacked mobile card with the row action (Mark as complete / Reopen) on its own line; canceled tasks get no row action at any width
plane: experiential
question: How does /company/tasks lay out a task row and its action at a 375px phone viewport, and which tasks have no row action?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 375
anchors:
  - coordinate: /company/tasks
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T09:42:40.232Z
    by: session:ae74f8af
    who: Lenajava1
---
Storefront theme 2.60.0-pr-2536 (sales rep, Today scope). At 375x812 the desktop task table is replaced by stacked mobile cards (class sales-rep-task-list__mobile-item): title button, 'Due <date> · <task type>' meta, status chip, notes, then the row action as a separate ghost button on its own line (38px tall, label on one line, icon centred within 0.5px of the text). Measured in en, ru and de: no horizontal document scroll (documentElement.scrollWidth equals clientWidth), no status chip truncated. Because the action sits on its own line at this width, the desktop status-column width does not constrain it. A canceled task (isActive false, not completed) renders no row action: no button on the mobile card, and an empty action cell in the desktop table at 1440px.
