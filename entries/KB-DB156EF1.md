---
id: KB-DB156EF1
subject: A storefront hard load of an unknown path is a soft 404 and records one broken link per permalink whose HitCount rises by one per unresolved request
plane: experiential
question: Does a storefront miss record a broken link, and how does its HitCount change on repeat visits or xAPI slugInfo calls?
status: active
appliesTo:
  - axis: identity
    value: anonymous
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/seo/broken-links/search
  - coordinate: Query.pageContext
  - coordinate: Query.slugInfo
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T21:22:30.973Z
    by: session:f9a8159e
---
2026-10-05, theme 2.58.0, Seo 3.1006.0-alpha.101 and Catalog 3.1049.0-pr-910 (PR builds of the Debug SEO rework), store B2B-store, anonymous. Hard-loading a never-used path: the document GET returned HTTP 200 (the SPA shell) and the page rendered '404 / Page not found / An error occurred. Should we take you back to home page?' with tab title '<store> 404 Page not found'. Exactly one request carried the permalink: GetPageContext (permalink without the leading slash) returned slugInfo.entityInfo null and redirectUrl null. 40 s later POST /api/seo/broken-links/search (keyword + storeId) returned one record: permalink without the slash, storeId B2B-store, language en-US, status Active, hitCount 1, createdBy http:anonymous, lastHitDate within one second of the request. A second hard load raised the same record to hitCount 2; no second record appeared. Separately, one anonymous xAPI slugInfo call plus one hard load of another unresolvable permalink moved its existing record from hitCount 5 to 7, so an unresolved xAPI slugInfo counts a hit just as a hard load does. A test that counts broken links must therefore count every unresolved resolution call, not only page visits.
