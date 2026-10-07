---
id: KB-1AC3D57E
subject: On /company/tasks, keyboard focus drops to the document body after a row action (Mark as complete / Reopen), after Edit task Save and after Delete; Escape on Edit task returns focus to the task title
plane: experiential
question: Where does keyboard focus go on /company/tasks after Mark as complete, Reopen, Edit task Save, Delete, or Escape from Edit task?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Mutation.changeSalesRepTaskStatus
  - coordinate: Mutation.updateSalesRepTask
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:19:15.785Z
    by: session:5e2a1103
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:59:37.990Z
    by: session:5e2a1103
    who: Lenajava1
    note: "Re-observed at 1440 in regression run: keyboard Enter on Mark as complete and on Reopen both leave [active] on the document root; next Tab lands on the first row title."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T13:32:41.011Z
    by: session:5e2a1103
    who: Lenajava1
    note: "Re-observed in Edge 1920 on a self-created task: Enter on Mark as complete, Enter on Reopen, Edit task Save (keyboard) and Delete then OK each leave document.activeElement = BODY. The pre-redesign /company/calendar page (same backend) behaves the same for checkbox toggle, Edit Save and Delete, so this predates the Tasks redesign."
---
Theme 2.59.0-pr-2536, sales rep signed in, Chrome 1920. Tab to a row's 'Mark as complete' or 'Reopen' button and press Enter: Mutation.changeSalesRepTaskStatus is sent once, the list and counts refetch, and the accessibility snapshot shows [active] on the document root. The next Tab lands on the FIRST row's title, not on the acted row. A mouse click gives the same result. Opening Edit task with Enter on a title and pressing Save (Mutation.updateSalesRepTask) also leaves focus on the root. Deleting from Edit task (Delete, then OK) leaves document.activeElement = BODY. Escape from Edit task returns focus to the title button that opened it, and creating a task returns focus to the New task button. Reproduced 2026-10-07 on several seeded tasks.
