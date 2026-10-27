---
id: KB-C27D9FF1
subject: "'Edit task' on a non-call task lets the rep change Type to Call, which adds Customer and Time (no Add to calendar); once saved the task reopens as 'Edit call' with no Type, so the change is one-way"
plane: experiential
question: Can the Edit task dialog on /company/tasks turn an existing non-call task into a Call, and can it be turned back?
questions:
  - text: Can a task's type be changed to Call after creation?
  - text: Why can I not change a call back to another type?
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
    deployment: vcptcore_qa
    build: 956cfff4cb0a606f
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; browser=firefox; role=Sales Representative
    at: 2026-10-27T10:33:47.744Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
    note: "distinct: KB-4A03C0E8 covers the New task (create) popup and KB-7C2F8AB2 editing an existing call; this is converting an EXISTING non-call task to Call through Edit task, and the one-way result (no Type once it is a call)."
---
A type Other task (no customer) opened 'Edit task' with Title, Type (with Clear), Due date, Priority, Notes. Choosing Call in Type added Customer (empty) and Time 9:00 AM; the heading stayed 'Edit task' and no 'Add to calendar' group appeared. Save sent Mutation.updateSalesRepTask {type:"Call", dueDate:"2026-10-28T06:00:00.000Z", no customerId key} and returned type Call, customerId null. Reopening the task showed 'Edit call' with Customer, Due date, Time, Priority — no Type select, so it cannot be switched back from the storefront.
