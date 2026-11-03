---
id: KB-DBF2D084
subject: With index swap supported, the search index REST API offers no way to remove a document type's active index
plane: experiential
question: GET /api/search/indexes and POST /api/search/indexes (Build, deleteExistingIndex) — how to read the index state, and can a document type be left with no index to test a missing-index path?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/search/indexes
  - coordinate: POST /api/search/indexes
  - coordinate: GET /api/search/indexes/index/{id}/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: da77a4958c725fef
    conditions: module:VirtoCommerce.ElasticSearch8=3.1012.0-pr-47-d205; swapIndexSupported=true
    at: 2026-11-03T14:23:41.044Z
    by: session:ef5e0d1c
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-11-03T20:19:32.270Z
    by: session:ef5e0d1c
    bodyHash: a8aa5d62be3e
    who: Lenajava1
    note: "2026-11-03: Product Delete and build built into the backup slot then swapped; anonymous xAPI sampled every 10 s during it never returned 0 or errors; previous index remained as backup row."
---
On an Elasticsearch 8 stand with swapIndexSupported=true, GET /api/search/indexes lists every registered document type with an active index (and a backup index). POST /api/search/indexes with deleteExistingIndex (Delete and build) builds into the backup index and then swaps aliases, so the active index is never absent during or after the build. The search swagger has no route that drops an index. So a registered document type cannot be put into a no-index state through the platform API; a missing-index path can only be reached by deleting the index directly in Elasticsearch. GET /api/search/indexes/index/{documentType}/{documentId} for an unregistered document type returns 200 with [] (no error).
