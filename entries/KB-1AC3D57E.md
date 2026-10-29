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
    build: b7643117cf5b609f
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:59:37.990Z
    by: session:5e2a1103
    who: Lenajava1
    note: "Re-observed at 1440 in regression run: keyboard Enter on Mark as complete and on Reopen both leave [active] on the document root; next Tab lands on the first row title."
    build: b7643117cf5b609f
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T13:32:41.011Z
    by: session:5e2a1103
    who: Lenajava1
    note: "Re-observed in Edge 1920 on a self-created task: Enter on Mark as complete, Enter on Reopen, Edit task Save (keyboard) and Delete then OK each leave document.activeElement = BODY. The pre-redesign /company/calendar page (same backend) behaves the same for checkbox toggle, Edit Save and Delete, so this predates the Tasks redesign."
    build: b7643117cf5b609f
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: module:VirtoCommerce.SalesRep=3.1014.0; theme=2.60.0-alpha.2579; viewport=1440x1080; browser=edge
    at: 2026-10-15T13:27:31.030Z
    by: session:10db9d7d
    bodyHash: d7e1ff970269
    defect: suspected
    who: Lenajava1
    note: "Re-observed on Edge 1440 with salesrep 3.1014.0 release: Enter on Mark as complete and on Reopen leaves [active] on the document root; next Tab lands on the first row title."
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; theme=2.61.0-pr-2576-be2c; browser=firefox; viewport=1920x1080
    at: 2026-10-27T19:54:45.878Z
    by: session:e75fe8a4
    bodyHash: d7e1ff970269
    who: Aleksandra-Mitricheva
    note: "Re-observed in Firefox: Enter on Mark as complete and on Reopen leaves [active] on document root; next Tab lands on the first row title."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605; browser=firefox; viewport=1440x1080; role=sales-rep
    at: 2026-10-29T09:10:30.271Z
    by: session:ce3cfc2a
    bodyHash: d7e1ff970269
    defect: suspected
    who: Aleksandra-Mitricheva
    note: "Re-observed keyboard-only in Firefox 1440: Enter on Mark as complete and on Reopen, and Edit task Save, each leave document.activeElement = BODY; Delete -> OK too. Next Tab after a row action lands on the first row's title; after Edit Save it lands on the page's first control (Skip to main content). Escape from Edit returns focus to the title; create returns it to New task. Instrumented timing (Reopen): ~880 ms after Enter the acted button gets disabled (the list's busy prop while the mutation runs) and focusout fires 5 ms later with relatedTarget null; ~400 ms later the button is re-enabled, then the table body is swapped for the loading skeleton and back, so the acted node is detached and a new node renders. Two independent focus-loss points; the page restores focus after neither (sales-rep module StorefrontApp sales-rep-tasks-page.vue toggleCompletion/onTaskSaved, read at tag 3.1016.0)."
---
Theme 2.59.0-pr-2536, sales rep signed in, Chrome 1920. Tab to a row's 'Mark as complete' or 'Reopen' button and press Enter: Mutation.changeSalesRepTaskStatus is sent once, the list and counts refetch, and the accessibility snapshot shows [active] on the document root. The next Tab lands on the FIRST row's title, not on the acted row. A mouse click gives the same result. Opening Edit task with Enter on a title and pressing Save (Mutation.updateSalesRepTask) also leaves focus on the root. Deleting from Edit task (Delete, then OK) leaves document.activeElement = BODY. Escape from Edit task returns focus to the title button that opened it, and creating a task returns focus to the New task button. Reproduced 2026-10-07 on several seeded tasks.
