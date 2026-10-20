---
id: KB-CE9257BC
subject: Saving Edit task on a canceled sales-rep task succeeds; updateSalesRepTask answers "Task not found." only when the task is gone or not the rep own
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
    build: 850daf039b204244
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T11:51:49.732Z
    by: session:5e2a1103
    who: Lenajava1
    contradicts: true
    note: "Retraction by the original author: the \"Task not found.\" error was a concurrent-delete race. The canceled task used was a temporary probe row that a parallel seeding run deleted within the same minute (the All count fell 16 to 14 straight afterwards). Re-tested on a properly seeded canceled task (isActive:false, completed:false) on /company/tasks: an unchanged Edit task save sends Mutation.updateSalesRepTask and returns HTTP 200 with the task in data, errors[] empty. The task stays canceled (isActive:false, completed:false). The modal closes and the list, counts and markers refetch. Saving a canceled task succeeds; the entry's claim does not hold."
    resolved: claim-amended
    resolvedAt: 2026-10-12T18:06:13.298Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "The claim is wrong: on the current build a canceled task saves (4/4 on vcst) and the deployed SalesRep throws 'Task not found.' only for a missing or foreign task; the entry's error is reproduced by deleting the task while Edit is open, the race the disputes name."
    amendedFrom: 7ef889262b46
    build: 850daf039b204244
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:18:59.251Z
    by: session:5e2a1103
    who: Lenajava1
    contradicts: true
    note: "Independent re-check on a second seeded canceled task (isActive:false, completed:false, due +2 days): opening Edit task on /company/tasks and saving unchanged sent Mutation.updateSalesRepTask, HTTP 200, data returned, no errors[]. The task stayed isActive:false, completed:false, kept no row action, and the counts and list refetched. Saving a canceled task succeeds."
    resolved: claim-amended
    resolvedAt: 2026-10-12T18:06:15.382Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "The claim is wrong: on the current build a canceled task saves (4/4 on vcst) and the deployed SalesRep throws 'Task not found.' only for a missing or foreign task; the entry's error is reproduced by deleting the task while Edit is open, the race the disputes name."
    amendedFrom: 7ef889262b46
    build: b7643117cf5b609f
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:59:38.030Z
    by: session:5e2a1103
    who: Lenajava1
    contradicts: true
    note: "Freshly seeded canceled task (isActive:false, completed:false, past due): Edit task unchanged Save sent Mutation.updateSalesRepTask, HTTP 200, data returned with isActive:false completed:false, no errors[], toast 'Task saved'; a second save re-dating it +20 days also succeeded and the task stayed canceled with no row action."
    resolved: claim-amended
    resolvedAt: 2026-10-12T18:06:17.334Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "The claim is wrong: on the current build a canceled task saves (4/4 on vcst) and the deployed SalesRep throws 'Task not found.' only for a missing or foreign task; the entry's error is reproduced by deleting the task while Edit is open, the race the disputes name."
    amendedFrom: 7ef889262b46
    build: b7643117cf5b609f
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511-81f1; module:VirtoCommerce.SalesRep=3.1013.0-pr-18-d367; module:VirtoCommerce.TaskManagement=3.1005.0; role=sales-rep
    at: 2026-10-12T18:06:10.384Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-12: 4 fresh canceled tasks (isActive:false, completed:false): unchanged Save 3/3 and a re-dated Save 1/1 -> UpdateSalesRepTask HTTP 200, data, no errors[], 'Task saved', modal closes, task stays canceled. Completed task saves too. Canceled row: no action; completed: Reopen. Deleting a canceled task while its Edit modal is open, then Save -> errors 'Task not found.', data null, 'Something went wrong. Please try again later.', modal stays open. Source: SalesRepTaskCommandHandlerBase throws only when task is null or not the rep's."
    investigation: true
    bodyHash: 3b83d8af725d
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: module:VirtoCommerce.SalesRep=3.1014.0; theme=2.60.0-alpha.2579; browser=edge
    at: 2026-10-15T13:28:38.165Z
    by: session:10db9d7d
    bodyHash: 3b83d8af725d
    who: Lenajava1
    note: "Re-observed after salesrep 3.1014.0 release: canceled Kilo unchanged Save and re-dated Save (+20 days) -> UpdateSalesRepTask 200, no errors, isActive:false completed:false, 'Task saved', no row action, counts unchanged."
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; module:VirtoCommerce.TaskManagement=3.1005.0; theme=2.60.0; role=Sales Representative
    at: 2026-10-20T14:29:16.450Z
    by: session:70c763de
    bodyHash: 3b83d8af725d
    who: yuskithedeveloper
    note: "Foreign-task half re-observed with a second rep's own token on /graphql/sales-rep: updateSalesRepTask, changeSalesRepTaskStatus and deleteSalesRepTask on another rep's task each return errors[{message:\"Task not found.\"}] with data null, and the owner's task is unchanged (still present, same priority/dueDate/customer). Query.salesRepTask on that id returns null without an error and salesRepTasks lists 0."
---
On /company/tasks a canceled task (isActive:false, completed:false) renders a clickable title that opens the Edit task modal with every field editable and Save enabled. Saving it, unchanged or with a new due date, sends Mutation.updateSalesRepTask and returns HTTP 200 with the task in data and no errors[]; the toast is 'Task saved', the modal closes, the list and counts refetch, and the task stays canceled. A completed task saves the same way. The row offers no action for a canceled task and 'Reopen' for a completed one. updateSalesRepTask answers HTTP 200 with errors[{message:"Task not found."}] and data null only when the task no longer exists or is not the rep's own (SalesRepTaskCommandHandlerBase, one answer for both so a write cannot probe ids); the storefront then shows 'Something went wrong. Please try again later.' and keeps the modal open. Deleting the task while its Edit modal is open reproduces exactly that. Observed 2026-10-12 on theme 2.60.0-pr-2511-81f1 with SalesRep 3.1013.0-pr-18-d367 and TaskManagement 3.1005.0 (vcst); the completed-task save also on vcptcore.
