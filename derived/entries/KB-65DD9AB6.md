---
id: KB-65DD9AB6
subject: rest-api-assetentries
plane: derived-first
question: Which endpoints does this deployment serve under /api/assetentries, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Assets
    version: 3.1001.2
anchors:
  - coordinate: DELETE /api/assetentries
    operationId: AssetEntry_Delete
    hash: 34d76c53056b
  - coordinate: GET /api/assetentries/{id}
    operationId: AssetEntry_Get
    hash: 05e3b3348ab1
  - coordinate: POST /api/assetentries/search
    operationId: AssetEntry_Search
    hash: 494d2c9fa509
  - coordinate: PUT /api/assetentries
    operationId: AssetEntry_Update
    hash: acf80ba87733
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/assetentries

4 operations under `/api/assetentries`, served by module `VirtoCommerce.Assets`, published under the tag "Assets Management".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/assetentries`<br>`AssetEntry_Delete` | Delete asset entries by ids | — | 204 |
| `GET /api/assetentries/{id}`<br>`AssetEntry_Get` | Get asset details by id | `id` (path) | `AssetEntry` |
| `POST /api/assetentries/search`<br>`AssetEntry_Search` | SearchAsync for AssetEntries by AssetEntrySearchCriteria | body application/json (optional) | `AssetEntrySearchResult` |
| `PUT /api/assetentries`<br>`AssetEntry_Update` | Create / Update asset entry | body application/json (optional) | 204 |

Module `VirtoCommerce.Assets` — Assets Management: Common abstractions for asset search, retrieval, and manipulation, making it easy for developers to work with assets regardless of their underlying storage location.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-assetentries.json`.
