---
id: KB-8FBC1C7D
subject: rest-api-seo
plane: derived-first
question: Which endpoints does this deployment serve under /api/seo, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Seo
    version: 3.1000.1
anchors:
  - coordinate: DELETE /api/seo/broken-links
    operationId: BrokenLinks_Delete
    hash: 1566118f10f7
  - coordinate: DELETE /api/seo/redirect-rules
    operationId: RedirectRules_Delete
    hash: 76cef2adfa11
  - coordinate: GET /api/seo/broken-links/{id}
    operationId: BrokenLinks_GetById
    hash: 6aee530dd1b7
  - coordinate: GET /api/seo/redirect-rules/{id}
    operationId: RedirectRules_GetById
    hash: dc5f556199b0
  - coordinate: PATCH /api/seo/broken-links/{id}
    operationId: BrokenLinks_PatchAssociation
    hash: 4189eeeb9c57
  - coordinate: PATCH /api/seo/redirect-rules/{id}
    operationId: RedirectRules_PatchAssociation
    hash: 0cb7792bb72c
  - coordinate: POST /api/seo/broken-links
    operationId: BrokenLinks_Create
    hash: 4b4f9009c7b7
  - coordinate: POST /api/seo/broken-links/search
    operationId: BrokenLinks_Search
    hash: 5009d1fe61ef
  - coordinate: POST /api/seo/redirect-rules
    operationId: RedirectRules_Create
    hash: 4b1d8a75bd89
  - coordinate: POST /api/seo/redirect-rules/search
    operationId: RedirectRules_Search
    hash: 4d96d78b5c3e
  - coordinate: PUT /api/seo/broken-links
    operationId: BrokenLinks_Update
    hash: 94e91244b27e
  - coordinate: PUT /api/seo/redirect-rules
    operationId: RedirectRules_Update
    hash: 63c4d0f89efb
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/seo

12 operations under `/api/seo`, served by module `VirtoCommerce.Seo`, published under the tag "SEO".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/seo/broken-links`<br>`BrokenLinks_Delete` | — | — | 204 |
| `DELETE /api/seo/redirect-rules`<br>`RedirectRules_Delete` | — | — | 204 |
| `GET /api/seo/broken-links/{id}`<br>`BrokenLinks_GetById` | — | `id` (path) | `BrokenLink` |
| `GET /api/seo/redirect-rules/{id}`<br>`RedirectRules_GetById` | — | `id` (path) | `RedirectRule` |
| `PATCH /api/seo/broken-links/{id}`<br>`BrokenLinks_PatchAssociation` | — | `id` (path), body `Operation[]` (optional) | 204 |
| `PATCH /api/seo/redirect-rules/{id}`<br>`RedirectRules_PatchAssociation` | — | `id` (path), body `Operation[]` (optional) | 204 |
| `POST /api/seo/broken-links`<br>`BrokenLinks_Create` | — | body application/json (optional) | `BrokenLink` |
| `POST /api/seo/broken-links/search`<br>`BrokenLinks_Search` | — | body application/json (optional) | `BrokenLinkSearchResult` |
| `POST /api/seo/redirect-rules`<br>`RedirectRules_Create` | — | body application/json (optional) | `RedirectRule` |
| `POST /api/seo/redirect-rules/search`<br>`RedirectRules_Search` | — | body application/json (optional) | `RedirectRuleSearchResult` |
| `PUT /api/seo/broken-links`<br>`BrokenLinks_Update` | — | body application/json (optional) | 204 |
| `PUT /api/seo/redirect-rules`<br>`RedirectRules_Update` | — | body application/json (optional) | 204 |

Module `VirtoCommerce.Seo` — SEO: Infrastructure for managing SEO metadata across the platform

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-seo.json`.
