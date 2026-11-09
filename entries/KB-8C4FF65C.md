---
id: KB-8C4FF65C
subject: The Admin Returns list status filter and POST /api/return/search fold the Cancelled and Canceled spellings into one status
plane: experiential
question: Does filtering returns by Cancelled also return legacy returns stored as Canceled?
questions:
  - text: Why does the Cancelled filter in the Admin Returns list show a row whose status reads Canceled?
  - text: Does POST /api/return/search statuses [Canceled] return Cancelled returns too?
status: active
appliesTo:
  - axis: feature
    value: returns
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/return/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 0a0b8dac8ab16bcc
    conditions: platform=3.1076.8; module:VirtoCommerce.Return=3.1004.0; setting:Return.Status=10 values incl. Canceled and Cancelled; role=Administrator
    at: 2026-11-09T13:57:57.750Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
---
2026-11-09, Return 3.1004.0, Return.Status dictionary holding both spellings: POST /api/return/search with statuses ["Cancelled"] and with statuses ["Canceled"] both answered totalCount 42 with results whose stored status is either Cancelled or Canceled (one legacy row RET231002-00001 stored Canceled). The Admin SPA Return list status filter lists "Cancelled" once; choosing it shows 42 rows, the legacy one displaying "Canceled" in the Return status column. So the search treats the two spellings as one status; a grid row may still display the legacy spelling under the Cancelled filter.
