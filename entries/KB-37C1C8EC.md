---
id: KB-37C1C8EC
subject: The sales rep task modal shows "Task saved" for 5 seconds after a successful create or edit
plane: experiential
question: "/company/tasks and Schedule call: after a successful Save, which notice does the sales rep plugin show, and for how long?"
questions:
  - text: "createSalesRepTask Save in the sales rep task modal: is a success toast shown?"
  - text: what does the sales rep task popup show when the call is saved but adding it to the calendar fails?
  - text: does a failed createSalesRepTask or updateSalesRepTask show a message from the task modal itself?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: /company/my-customers/{id}
  - coordinate: Mutation.createSalesRepTask
  - coordinate: Mutation.updateSalesRepTask
evidence:
  - method: source
    deployment: vc-module-sales-rep@7007756
    at: 2026-10-20T16:35:01.137Z
    by: session:70c763de
    who: yuskithedeveloper
    note: "distinct: KB-590935DA is about the Notes textarea's accessible name; this entry is about the notice the modal shows after Save (its text, its 5-second duration and the calendar-failure warning)."
---
After a successful createSalesRepTask or updateSalesRepTask, the sales rep task modal calls useNotifications().success with "Task saved" (duration 5000 ms, single). When a new call saves but its calendar step fails (the Google tab is blocked or the .ics step throws), the modal shows the warning "Task saved, but it could not be added to the calendar" instead. An edit always shows "Task saved", because an edit offers no calendars. A failed write adds no notice of its own: the only message is the global GraphQL error toast from useMutation. Read in StorefrontApp sales-rep-task-modal.vue (save handler) and locales/en.json. Not observed live: a screenshot taken after a Save on a local stand showed no notice, which the 5-second auto-dismiss can explain.
