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
  - method: observation
    deployment: vcptcore_qa
    build: 956cfff4cb0a606f
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; browser=firefox; browser-timezone=UTC+03:00
    at: 2026-10-27T10:33:39.305Z
    by: session:e75fe8a4
    bodyHash: 1d4fd6ccd369
    who: Aleksandra-Mitricheva
    note: "UTC+3: unchanged Save on 'Edit call' sent dueDate \"2026-10-28T11:30:00Z\" exactly as stored with the same customerId; changing Time 2:30 PM→4:45 PM sent and returned 2026-10-28T13:45:00Z, row \"Due Oct 28, 2026, 4:45 PM · <customer> · Call\". Edit call has no Type and no Add to calendar."
---
Theme 2.60.0 with the SalesRep plugin built from vc-module-sales-rep PR #26 (unreleased), browser zone UTC-6. Clicking a call's title on /company/tasks opens 'Edit call' with Title, Customer, Due date, Time (the stored local time), Priority, Notes and Delete / Cancel / Save; there is no Type field and no 'Add to calendar'. Changing only the Time from 2:30 PM to 3:45 PM sent Mutation.updateSalesRepTask with dueDate 2026-10-23T21:45:00.000Z and the same customerId; a later notes-only edit sent dueDate '2026-10-23T21:45:00Z' exactly as stored (no milliseconds added) with the same customerId. Each response returned the call with the linked organization's customerName, and the list kept 'Due Oct 23, 2026, 3:45 PM · Contoso Ltd. · Call'.
