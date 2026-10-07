---
id: KB-CE9257BC
subject: "Saving Edit task on a canceled sales-rep task fails: updateSalesRepTask returns errors[] \"Task not found.\" inside HTTP 200"
plane: experiential
question: What happens when a sales rep opens and saves Edit task on a canceled task on /company/tasks?
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
    deployment: vcst_qa
    at: 2026-10-07T11:45:54.325Z
    by: session:5e2a1103
    who: Lenajava1
---
On /company/tasks (storefront theme 2.59.0-pr-2536 against SalesRep 3.1012.0 / TaskManagement 3.1005.0), a canceled task (isActive:false, completed:false) still renders a clickable title that opens the Edit task modal with every field editable and Save enabled. Saving it unchanged sends Mutation.updateSalesRepTask with the task id; the response is HTTP 200 with errors[{message:"Task not found."}] and data.updateSalesRepTask null. The storefront shows the generic toast "Something went wrong. Please try again later." and the modal stays open. The same unchanged save on a completed task (isActive:false, completed:true) succeeds with no errors, so the server rejects only the canceled state. The row itself offers no Mark as complete / Reopen action for canceled.
