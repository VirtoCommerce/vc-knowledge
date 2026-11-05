---
id: KB-E3CD12FA
subject: On the PR #33 build the Tasks page replaces the Today/All scope chips and status tabs with a search box, Customers, Status and Priorities selects, filter chips and Reset filters, and adds Priority and Customer columns
plane: experiential
question: What does /company/tasks show after the Customer and Priority filters change (columns, filters, chips, empty states, deep link ?filter=overdue)?
questions:
  - text: Does the Tasks page still have Today and All chips?
  - text: Where did the status tabs go on /company/tasks?
  - text: What does the dashboard overdue link open on the Tasks page?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Query.salesRepTasks
  - coordinate: Query.salesRepTaskFilterRules
evidence:
  - method: observation
    deployment: vcst_qa
    build: 29f9d60e551ad495
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-8d4a; module:VirtoCommerce.TaskManagement=3.1007.0-pr-33-14ff; theme=2.62.0-pr-2638-1dba-1dbab4e2; platform=3.1094.0; browser=edge
    at: 2026-11-05T13:24:58.277Z
    by: session:ffa9e791
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: platform=3.1094.0; module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; storefront-plugin-bundle=last-modified 12:23:20; theme=2.62.0-pr-2637-02f2-02f26436; browser=edge
    at: 2026-11-05T15:26:21.185Z
    by: session:ffa9e791
    bodyHash: d3c13e0eacd8
    who: Lenajava1
    note: "Same layout, columns, selects and chips seen 2026-11-05 15:05-15:25Z. Nuance for the no-org rep: the Status combobox itself is NOT disabled; it opens and lists one disabled option 'No options available'. The New task dialog for that rep has no Type field and its Customer list is also 'No options available'."
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; module:VirtoCommerce.TaskManagement=3.1007.0-pr-33-14ff; storefront-plugin-bundle=last-modified 12:23:20; browser=chromium
    at: 2026-11-05T16:19:39.284Z
    by: session:5b90a3e1
    bodyHash: d3c13e0eacd8
    who: Lenajava1
    note: "Chrome lane 15:56-16:19Z: same layout, selects, options, chips and columns; ?filter=overdue opens on Overdue only (2 seeded overdue), URL cleaned; ?filter=zzz settles to no chips and every task; picks within one axis union, across axes intersect; High never matches Highest, Lowest never Low; Reset filters keeps the search term; chip order day, customers, status, priorities."
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; module:VirtoCommerce.TaskManagement=3.1007.0-pr-33-14ff; storefront-plugin-chunk=sales-rep-tasks-page-Dss09knJ; browser=firefox
    at: 2026-11-05T16:53:49.061Z
    by: session:5b90a3e1
    bodyHash: d3c13e0eacd8
    who: Lenajava1
    note: "Firefox lane on the ff13 chunk: dashboard '2 overdue tasks' notice -> /company/tasks?filter=overdue opens on Overdue only with 2 tasks, URL cleaned; Reset filters drops the chip row; Customer+Priority intersect; a task saved via New task with a Customer (type Other) is found by that customer's filter. The New task dialog has no time field: a task saved for today is stored with dueDate = local midnight."
  - method: observation
    deployment: vcst_qa
    build: c1947adbe472459a
    conditions: platform=3.1094.0; module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; module:VirtoCommerce.TaskManagement=3.1007.0-pr-33-14ff; theme=2.62.0-pr-2637-02f2-02f26436; browser=chromium; role=Sales Representative
    at: 2026-11-05T17:34:15.137Z
    by: session:26f3f3df
    bodyHash: d3c13e0eacd8
    who: Lenajava1
    note: "2026-11-05 ~17:25Z, Chrome lane at 1920x1080, rep serving 5 orgs: page opens with a Due today chip; removing it lists every date; Customers lists exactly the 5 served orgs; one customer pick adds its chip and narrows list and calendar dots; adding Priorities=Highest intersects (2 -> 1 task); New task's Customer select shows placeholder 'No customer' and lists the served orgs; a task saved with a customer shows it in the Customer column and is found by that customer's filter at once."
---
Signed in as a sales rep serving 5 organizations on a stand with SalesRep 3.1018.0-pr-33-8d4a and TaskManagement 3.1007.0-pr-33-14ff, /company/tasks renders: H1 Tasks, a New task button, a "Search tasks" box, three selects (Customers: the 5 served organizations, Status: Upcoming / Overdue / Completed, Priorities: Lowest / Low / Normal / High / Highest), a chip row that opens with "Due today" (with a remove button) plus a "Reset filters" button, a "N tasks" status line, and a table with columns Task, Priority, Customer, Status, Notes, Actions (customer cell is an em dash when the task has none; overdue rows read "Expired <date>", others "Due <date>"). There are no Today/All scope chips and no Upcoming/Overdue/Completed tabs with counts. Reset filters removed the Due today chip and showed all 24 tasks on 2 pages (Previous/Next); picking the AcmeCorp customer added a chip and reduced the list to 1 task. /company/tasks?filter=overdue opens with the Status select set to Overdue (chip "Overdue"), no day chip, 10 tasks, and the URL is cleaned to /company/tasks. A rep with 1 org and 0 tasks sees "0 tasks / Nothing due on this day" under the Due today chip; a rep with no organizations sees the same page but its Status select lists only a disabled "No options available". The New task dialog has a Customer select for every task (not only calls), blank by default, offering the rep's served organizations only.
