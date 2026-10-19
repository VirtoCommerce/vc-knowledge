---
id: KB-CB5C5AD3
subject: Parallel saves of one localizable dictionary setting can return 409 and can persist exact-duplicate translation rows, which GET lists and the next save collapses
plane: experiential
question: What happens when several POST /api/platform/localizable-settings/{name}/dictionary-items requests for the same setting run concurrently?
questions:
  - text: Can concurrent saves of Order.Status translations create duplicate translation rows?
  - text: Does GET /api/platform/localizable-settings show duplicate localizedValues for one alias and language?
  - text: Why does saving a localizable setting return 409 with an optimistic concurrency message?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/localizable-settings/{id}/dictionary-items
  - coordinate: GET /api/platform/localizable-settings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1c613e804eb731ee
    conditions: platform=3.1086.0-pr-3139-5d63; setting=Order.Status; role=Administrator
    at: 2026-10-19T08:39:02.147Z
    by: session:f5d50e59
    who: Lenajava1
    note: "distinct: KB-74D9B9B0 is about stored aliases differing only in case making every save 500; this entry is about what concurrent saves do (409 optimistic-concurrency responses, exact same-alias duplicate rows created and visible in GET) on a build where no 500 occurs."
---
On platform 3.1086.0-pr-3139-5d63 (Order.Status), 10 parallel admin POSTs to /api/platform/localizable-settings/Order.Status/dictionary-items, each carrying the full current item set plus one new translation (alias Processing, es-ES), returned 6x204 and 4x409 with body {"message":"The database operation was expected to affect 1 row(s), but actually affected 0 row(s); ... optimistic concurrency exceptions ...","stackTrace":null}. Immediately afterwards GET /api/platform/localizable-settings listed the Processing item with THREE identical es-ES localizedValues, so exact-duplicate stored rows (same alias, same language) are visible through REST. One ordinary save of the GET output then returned 204 and the next GET showed a single es-ES value. The race is not deterministic: 12 further parallel bursts (10-20 requests, other languages) produced no duplicate rows, and one of them returned 2x409. No 500 was returned in any burst.
