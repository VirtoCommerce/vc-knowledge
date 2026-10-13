---
id: KB-A29DAAFA
subject: BackgroundJobs 3.1053 lists each same-named job handler as its own row named by full type; enqueue by the shared short name returns 404
plane: experiential
question: Does GET /api/background-jobs/registered list every module's LogEntityChangesJobHandler, and how is a shared short name resolved by POST /api/background-jobs/enqueue?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: /api/background-jobs/registered
  - coordinate: /api/background-jobs/enqueue
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-13T19:26:50.936Z
    by: session:3a76f237
    who: kutasinaelena
---
On BackgroundJobs 3.1053.0-pr-7-3bcd (Hangfire engine), GET /api/background-jobs/registered returned 65 rows where 3.1052.0 returned 57: the single LogEntityChangesJobHandler row became 9 rows, one per installed module (Catalog, CatalogPersonalization, Content, Customer, DynamicAssociations, Inventory, Marketing, Pricing, Store), each with name equal to its full handler type name and triggerable=false; unique names kept their short name and flags. POST /api/background-jobs/enqueue with {name:'LogEntityChangesJobHandler'} returned 404 'No registered background job named ...'; with a full type name it returned 400 'is internal and cannot be triggered on demand' (lower-cased full name resolved the same way). The Hangfire dashboard shows the handler name (e.g. IndexChangesJobHandler) and 'Recurring: <id>' as job names. GET /api/background-jobs/recurring returned lastRunUtc=null for every item while /hangfire/recurring showed recent last executions.
