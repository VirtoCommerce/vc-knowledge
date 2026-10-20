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
    build: b7643117cf5b609f
    buildFrom: deploy-history
  - method: observation
    deployment: local
    conditions: role=Sales Representative; state:browser-timezone=UTC-6
    at: 2026-10-20T14:33:15.807Z
    by: session:70c763de
    bodyHash: 01006f4744d1
    who: yuskithedeveloper
    note: "Browser zone UTC-6 on 2026-10-20, SalesRep plugin from PR #26 on theme 2.60.0: SalesRepTasks sent today=2026-10-20T06:00:00.000Z and period {from 2026-10-20T06:00:00.000Z, to 2026-10-21T05:59:59.999Z}; SalesRepTaskCounts carried the same today and todayPeriod. A picked day sends that day's local-midnight window (Oct 24: 2026-10-24T06:00:00.000Z to 2026-10-25T05:59:59.999Z), so a call at 23:45 local stays on its day and one at 00:00 lands on the next. The markers query sent first:200 over a padded window (2026-09-24T06:00Z to 2026-11-15T05:59:59.999Z), wider than the visible Sep 27 - Nov 7 grid."
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0 (source build of vc-module-sales-rep PR 26 @ 7007756, unreleased); theme=2.60.0 (vc-frontend source); browser-timezone=UTC-06:00; role=Sales Representative
    at: 2026-10-20T14:34:17.581Z
    by: session:70c763de
    bodyHash: 01006f4744d1
    who: yuskithedeveloper
    note: "Re-observed 2026-10-20 in a browser at UTC-06:00 on theme 2.60.0 (source): Today SalesRepTasks sent today=2026-10-20T06:00:00.000Z and period {from 2026-10-20T06:00:00.000Z, to 2026-10-21T05:59:59.999Z}; SalesRepTaskCounts the same today/todayPeriod; All sent today only, no period; a picked day (Oct 23) sent period 2026-10-23T06:00:00.000Z..2026-10-24T05:59:59.999Z; SalesRepTaskMarkers sent first:200 with period 2026-09-24T06:00:00.000Z..2026-11-15T05:59:59.999Z. All these go to POST /graphql."
---
Storefront theme 2.59.0-pr-2536, browser zone Europe/Budapest (CEST, UTC+2) on 2026-10-07. The Today list request SalesRepTasks carries today=2026-10-06T22:00:00.000Z and period {from: 2026-10-06T22:00:00.000Z, to: 2026-10-07T21:59:59.999Z}; SalesRepTaskCounts carries the same today and todayPeriod. Status tabs (filter=overdue/upcoming/completed) and All send today only, no period. The calendar request sends first:200 with a period spanning the visible 6-week grid. Tasks due 00:00 local and 23:30 local both land in Today and under Upcoming, not Overdue. Counts are requested once per page load and again only after a write, not on chip clicks.
