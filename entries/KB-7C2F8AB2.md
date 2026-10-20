---
id: KB-7C2F8AB2
subject: Editing a call on /company/tasks keeps its customer, and a notes-only edit sends the stored dueDate back unchanged
plane: experiential
question: Does Mutation.updateSalesRepTask from the storefront Edit call keep the call's customerId and dueDate when only the notes or the time change?
questions:
  - text: Does the storefront unlink a call's customer when the call is edited?
  - text: Can a call's type be changed in Edit call?
concepts:
  - id: sales-rep
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Mutation.updateSalesRepTask
evidence:
  - method: observation
    deployment: local
    conditions: role=Sales Representative; state:browser-timezone=UTC-6
    at: 2026-10-20T14:31:11.431Z
    by: session:70c763de
    who: yuskithedeveloper
---
Theme 2.60.0 with the SalesRep plugin built from vc-module-sales-rep PR #26 (unreleased), browser zone UTC-6. Clicking a call's title on /company/tasks opens 'Edit call' with Title, Customer, Due date, Time (the stored local time), Priority, Notes and Delete / Cancel / Save; there is no Type field and no 'Add to calendar'. Changing only the Time from 2:30 PM to 3:45 PM sent Mutation.updateSalesRepTask with dueDate 2026-10-23T21:45:00.000Z and the same customerId; a later notes-only edit sent dueDate '2026-10-23T21:45:00Z' exactly as stored (no milliseconds added) with the same customerId. Each response returned the call with the linked organization's customerName, and the list kept 'Due Oct 23, 2026, 3:45 PM · Contoso Ltd. · Call'.
