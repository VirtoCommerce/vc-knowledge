---
id: KB-F4AC37A8
subject: rest-api-marketing-contentpublications
plane: derived-first
question: Which endpoints does this deployment serve under /api/marketing/contentpublications, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Marketing
    version: 3.1000.1
anchors:
  - coordinate: DELETE /api/marketing/contentpublications
    operationId: MarketingModuleDynamicContent_DeleteDynamicContentPublications
    hash: 9e5e11ddadc9
  - coordinate: GET /api/marketing/contentpublications/{id}
    operationId: MarketingModuleDynamicContent_GetDynamicContentPublicationById
    hash: 2203ff7ab411
  - coordinate: GET /api/marketing/contentpublications/new
    operationId: MarketingModuleDynamicContent_GetNewDynamicPublication
    hash: 268278870888
  - coordinate: POST /api/marketing/contentpublications
    operationId: MarketingModuleDynamicContent_CreateDynamicContentPublication
    hash: 7e647dcb4a48
  - coordinate: POST /api/marketing/contentpublications/search
    operationId: MarketingModuleDynamicContent_DynamicContentPublicationsSearch
    hash: 0ada666ddebc
  - coordinate: PUT /api/marketing/contentpublications
    operationId: MarketingModuleDynamicContent_UpdateDynamicContentPublication
    hash: d9c8663624a4
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/marketing/contentpublications

6 operations under `/api/marketing/contentpublications`, served by module `VirtoCommerce.Marketing`, published under the tag "Marketing".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/marketing/contentpublications`<br>`MarketingModuleDynamicContent_DeleteDynamicContentPublications` | Delete a dynamic content publication objects | — | 204 |
| `GET /api/marketing/contentpublications/{id}`<br>`MarketingModuleDynamicContent_GetDynamicContentPublicationById` | Find dynamic content publication object by id | `id` (path) | `DynamicContentPublication` |
| `GET /api/marketing/contentpublications/new`<br>`MarketingModuleDynamicContent_GetNewDynamicPublication` | Get new dynamic content publication object | — | `DynamicContentPublication` |
| `POST /api/marketing/contentpublications`<br>`MarketingModuleDynamicContent_CreateDynamicContentPublication` | Add new dynamic content publication object to marketing system | body application/json (optional) | `DynamicContentPublication` |
| `POST /api/marketing/contentpublications/search`<br>`MarketingModuleDynamicContent_DynamicContentPublicationsSearch` | Search dynamic content items by given criteria | body application/json (optional) | `DynamicContentPublicationSearchResult` |
| `PUT /api/marketing/contentpublications`<br>`MarketingModuleDynamicContent_UpdateDynamicContentPublication` | Update an existing dynamic content publication object | body application/json (optional) | 204 |

Module `VirtoCommerce.Marketing` — Marketing: Marketing system with dynamic contents and promotions management

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-marketing-contentpublications.json`.
