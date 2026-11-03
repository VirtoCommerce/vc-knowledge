---
id: KB-E0AF1F41
subject: "ES8 provider: a failed GetMapping response throws SearchException and is not cached; a valid empty mapping still is"
plane: experiential
question: "Elasticsearch 8 provider LoadMappingAsync: what does search or indexing do when the index mapping load fails (503 with an empty body, connection reset, timeout) — error, or a cached empty mapping and 0 hits?"
concepts:
  - id: search-index
  - id: product-search
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: GET /api/search/indexes
  - coordinate: POST /api/search/indexes/index
  - coordinate: Query.products
evidence:
  - method: source
    deployment: vc-module-elastic-search-8@d205e1ac
    at: 2026-11-03T15:27:09.417Z
    by: session:ef5e0d1c
    who: Lenajava1
---
Driven through the real provider and Elastic transport against a scripted fake Elasticsearch: on module head d205e1ac (PR #47), a GetMapping that answers 503 with {} throws SearchException (inner TransportException) and caches nothing — the next call after the server recovers loads the real mapping, no restart. Connection reset and timeout throw SearchException with HttpRequestException / TaskCanceledException as inner. A valid 200 with no properties is still cached as an empty mapping. IndexAsync and CreateIndexAsync throw SearchException on a failed load. On the base (dev 3464de37): 503 {} returned and cached an empty mapping until restart (searches 0 hits, IndexAsync went on to _bulk against it); reset/timeout threw NullReferenceException at LoadMappingAsync. A missing index (HEAD 404) never reaches the mapping load on either: an empty mapping is returned and no _mapping request is sent. The SearchException message carries the Elasticsearch node URL (audit trail and the provider's existing "URL: …, Scope: …" suffix), the response body, and on reset/timeout the inner stack trace.
