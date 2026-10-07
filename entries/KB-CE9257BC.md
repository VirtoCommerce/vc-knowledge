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
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T11:51:49.732Z
    by: session:5e2a1103
    who: Lenajava1
    contradicts: true
    note: "Retraction by the original author: the \"Task not found.\" error was a concurrent-delete race. The canceled task used was a temporary probe row that a parallel seeding run deleted within the same minute (the All count fell 16 to 14 straight afterwards). Re-tested on a properly seeded canceled task (isActive:false, completed:false) on /company/tasks: an unchanged Edit task save sends Mutation.updateSalesRepTask and returns HTTP 200 with the task in data, errors[] empty. The task stays canceled (isActive:false, completed:false). The modal closes and the list, counts and markers refetch. Saving a canceled task succeeds; the entry's claim does not hold."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:18:59.251Z
    by: session:5e2a1103
    who: Lenajava1
    contradicts: true
    note: "Independent re-check on a second seeded canceled task (isActive:false, completed:false, due +2 days): opening Edit task on /company/tasks and saving unchanged sent Mutation.updateSalesRepTask, HTTP 200, data returned, no errors[]. The task stayed isActive:false, completed:false, kept no row action, and the counts and list refetched. Saving a canceled task succeeds."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:59:38.030Z
    by: session:5e2a1103
    who: Lenajava1
    contradicts: true
    note: "Freshly seeded canceled task (isActive:false, completed:false, past due): Edit task unchanged Save sent Mutation.updateSalesRepTask, HTTP 200, data returned with isActive:false completed:false, no errors[], toast 'Task saved'; a second save re-dating it +20 days also succeeded and the task stayed canceled with no row action."
---
On /company/tasks (storefront theme 2.59.0-pr-2536 against SalesRep 3.1012.0 / TaskManagement 3.1005.0), a canceled task (isActive:false, completed:false) still renders a clickable title that opens the Edit task modal with every field editable and Save enabled. Saving it unchanged sends Mutation.updateSalesRepTask with the task id; the response is HTTP 200 with errors[{message:"Task not found."}] and data.updateSalesRepTask null. The storefront shows the generic toast "Something went wrong. Please try again later." and the modal stays open. The same unchanged save on a completed task (isActive:false, completed:true) succeeds with no errors, so the server rejects only the canceled state. The row itself offers no Mark as complete / Reopen action for canceled.
