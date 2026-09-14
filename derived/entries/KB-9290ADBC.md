---
id: KB-9290ADBC
subject: rest-api-personalization
plane: derived-first
question: Which endpoints does this deployment serve under /api/personalization, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.CatalogPersonalization
    version: 3.1000.0
anchors:
  - coordinate: GET /api/personalization/taggeditem/{id}
    operationId: PersonalizationModule_GetTaggedItem
    hash: 6e0d720c8bb4
  - coordinate: GET /api/personalization/taggeditem/{id}/tags/count
    operationId: PersonalizationModule_GetTagsCount
    hash: befb7adfddc2
  - coordinate: POST /api/personalization/outlines/synchronization/cancel
    operationId: PersonalizationModule_CancelSynchronization
    hash: b5846570a31f
  - coordinate: POST /api/personalization/outlines/synchronize
    operationId: PersonalizationModule_RunOutlinesSynchronization
    hash: 0a7d06f09e27
  - coordinate: POST /api/personalization/search
    operationId: PersonalizationModule_Search
    hash: ade7ba5dad64
  - coordinate: PUT /api/personalization/taggeditem
    operationId: PersonalizationModule_UpdateTaggedItem
    hash: 279b70b9bea3
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/personalization

6 operations under `/api/personalization`, served by module `VirtoCommerce.CatalogPersonalization`, published under the tag "Catalog Personalization".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/personalization/taggeditem/{id}`<br>`PersonalizationModule_GetTaggedItem` | GET: api/personalization/taggeditem/{id} | `id` (path) | `TaggedItem` |
| `GET /api/personalization/taggeditem/{id}/tags/count`<br>`PersonalizationModule_GetTagsCount` | GET: api/personalization/taggeditem/{id}/tags/count | `id` (path) | `integer` |
| `POST /api/personalization/outlines/synchronization/cancel`<br>`PersonalizationModule_CancelSynchronization` | PUT: api/personalization/outlines/synchronization/cancel | body application/json (optional) | 204 |
| `POST /api/personalization/outlines/synchronize`<br>`PersonalizationModule_RunOutlinesSynchronization` | PUT: api/personalization/outlines/synchronize | — | `TaggedItemOutlineSyncPushNotification` |
| `POST /api/personalization/search`<br>`PersonalizationModule_Search` | POST: api/personalization/search | body application/json (optional) | `TaggedItemSearchResult` |
| `PUT /api/personalization/taggeditem`<br>`PersonalizationModule_UpdateTaggedItem` | PUT: api/personalization/taggeditem | body application/json (optional) | 204 |

Module `VirtoCommerce.CatalogPersonalization` — Catalog Personalization: Help businesses personalize their categories and products to specific User Groups

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-personalization.json`.
