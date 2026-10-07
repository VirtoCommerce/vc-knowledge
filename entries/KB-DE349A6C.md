---
id: KB-DE349A6C
subject: A Static redirect rule whose inbound starts with '/' never matches slugInfo for the slash-less permalink; it matches only when the permalink itself carries the slash
plane: experiential
question: Does a Static redirect rule with inbound '/path' redirect the storefront permalink 'path'?
status: active
appliesTo:
  - axis: module
    value: seo
  - axis: redirect-rule-type
    value: static
  - axis: surface
    value: graphql
anchors:
  - coordinate: Query.slugInfo
  - coordinate: POST /api/seo/redirect-rules
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T21:39:54.105Z
    by: session:f9a8159e
---
Created two active Static rules on one store via POST /api/seo/redirect-rules: inbound '/<random-a>' -> 'seed-books' and inbound '<random-b>' -> 'seed-books'. GraphQL slugInfo(storeId, cultureName, permalink: '<random-a>') returned entityInfo null and redirectUrl null, and wrote a broken-link record for '<random-a>'; slugInfo for '<random-b>' returned redirectUrl 'seed-books'. slugInfo for '/<random-a>' (with the slash) returned redirectUrl 'seed-books'. RedirectResolver compares the raw permalink (case-insensitive equality), and storefront permalinks carry no leading slash, so the documented examples written as '/abc' never fire for a storefront request.
