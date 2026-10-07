---
id: KB-DC2F1BF0
subject: POST /api/seoinfos/search with storeId and only slug (no permalink) returns [] and adds a hit to the store's newest Active broken link
plane: experiential
question: What happens when POST /api/seoinfos/search is called with a storeId but no permalink?
status: active
appliesTo:
  - axis: module
    value: seo
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/seoinfos/search
  - coordinate: POST /api/seo/broken-links/search
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T21:52:16.238Z
    by: session:f9a8159e
---
With storeId set and a never-used permalink, POST /api/seoinfos/search returned 200 [] and about 15 s later the store had a new Active broken link for that permalink, HitCount 1. Immediately after, with that record the store's newest Active broken link, the same endpoint called with storeId + languageCode + slug (a slug that exists) and no permalink returned 200 [] and the newest record's HitCount went from 1 to 2; no record with an empty permalink was created. So an event without a permalink is attributed to whichever Active broken link is newest.
