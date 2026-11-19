---
id: KB-3D2CF53D
subject: GET /api/site-builder-pages/languages returns a bare array of the store's culture codes in store order, with no default-language marker
plane: experiential
question: What does GET /api/site-builder-pages/languages?storeId= return, and does it mark the store's default language?
questions:
  - text: What does GET /api/site-builder-pages/user-groups return?
  - text: Do the Site Builder settings endpoints require authorization?
  - text: How does the Site Builder content copy endpoint answer for a missing source or a user without builder:update?
  - text: Does a Site Builder content POST without If-Match get 428?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/site-builder-pages/languages
  - coordinate: GET /api/site-builder-pages/user-groups
  - coordinate: POST /api/site-builder-pages/grouped/{id}/content/{id}
  - coordinate: POST /api/site-builder-pages/grouped/{id}/content
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    at: 2026-11-19T20:12:34.634Z
    by: session:07a3812a
    who: Lenajava1
---
Admin bearer token, B2B-store: GET /api/site-builder-pages/languages?storeId=B2B-store -> 200 ["pl-PL","sv-SE",...,"en-US","ru-RU","es-ES"] (15 codes, same order as the store's languages; the store defaultLanguage en-US is not marked or first). GET /api/site-builder-pages/user-groups -> 200 ["contract-contract2","Wholesaler","VIP"]. Both answer 401 without Authorization. POST /api/site-builder-pages/grouped/{target}/content/{source} copies the source draft content -> 204; 401 anonymous; 404 (empty body) for a non-existent source; 403 (empty body, target unchanged) for a back-office user without builder:update. POST /grouped/{id}/content without If-Match -> 428 'Read the page content and send its ETag in If-Match or in the content-json body before saving.'; with a stale If-Match -> 412 'The page content version does not match the current version.'. POST /grouped/archive?ids= -> 204 then publish-status {published:false,hasChanges:false}; anonymous archive -> 401.
