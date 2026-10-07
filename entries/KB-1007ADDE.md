---
id: KB-1007ADDE
subject: The /company/tasks Today list and counts send a period of browser-local midnight to +24h-1ms in UTC, with today equal to the period start
plane: experiential
question: What today and period values does /company/tasks send in Query.salesRepTasks and Query.salesRepTaskCounts for the Today scope?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Query.salesRepTasks
  - coordinate: Query.salesRepTaskCounts
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T12:59:38.069Z
    by: session:5e2a1103
    who: Lenajava1
---
Storefront theme 2.59.0-pr-2536, browser zone Europe/Budapest (CEST, UTC+2) on 2026-10-07. The Today list request SalesRepTasks carries today=2026-10-06T22:00:00.000Z and period {from: 2026-10-06T22:00:00.000Z, to: 2026-10-07T21:59:59.999Z}; SalesRepTaskCounts carries the same today and todayPeriod. Status tabs (filter=overdue/upcoming/completed) and All send today only, no period. The calendar request sends first:200 with a period spanning the visible 6-week grid. Tasks due 00:00 local and 23:30 local both land in Today and under Upcoming, not Overdue. Counts are requested once per page load and again only after a write, not on chip clicks.
