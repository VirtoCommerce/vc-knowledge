---
id: KB-9C4FD9E4
subject: rest-api-marketing-contentfolders
plane: derived-first
question: Which endpoints does this deployment serve under /api/marketing/contentfolders, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Marketing
    version: 3.1000.1
anchors:
  - coordinate: DELETE /api/marketing/contentfolders
    operationId: MarketingModuleDynamicContent_DeleteDynamicContentFolders
    hash: 1d4581529416
  - coordinate: GET /api/marketing/contentfolders/{id}
    operationId: MarketingModuleDynamicContent_GetDynamicContentFolderById
    hash: 017c5f2f3da7
  - coordinate: POST /api/marketing/contentfolders
    operationId: MarketingModuleDynamicContent_CreateDynamicContentFolder
    hash: 5fe2bfa6ade6
  - coordinate: PUT /api/marketing/contentfolders
    operationId: MarketingModuleDynamicContent_UpdateDynamicContentFolder
    hash: a32142e03b8d
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/marketing/contentfolders

4 operations under `/api/marketing/contentfolders`, served by module `VirtoCommerce.Marketing`, published under the tag "Marketing".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/marketing/contentfolders`<br>`MarketingModuleDynamicContent_DeleteDynamicContentFolders` | Delete a dynamic content folders | — | 204 |
| `GET /api/marketing/contentfolders/{id}`<br>`MarketingModuleDynamicContent_GetDynamicContentFolderById` | Find dynamic content folder by id | `id` (path) | `DynamicContentFolder` |
| `POST /api/marketing/contentfolders`<br>`MarketingModuleDynamicContent_CreateDynamicContentFolder` | Add new dynamic content folder | body application/json (optional) | `DynamicContentFolder` |
| `PUT /api/marketing/contentfolders`<br>`MarketingModuleDynamicContent_UpdateDynamicContentFolder` | Update an existing dynamic content folder | body application/json (optional) | 204 |

Module `VirtoCommerce.Marketing` — Marketing: Marketing system with dynamic contents and promotions management

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-marketing-contentfolders.json`.
