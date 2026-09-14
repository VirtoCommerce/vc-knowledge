---
id: KB-D3D8FBEF
subject: rest-api-brand-settings
plane: derived-first
question: Which endpoints does this deployment serve under /api/brand-settings, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Catalog
    version: 3.1002.9
anchors:
  - coordinate: GET /api/brand-settings/store/{storeId}
    operationId: CatalogModuleBrandSetting_GetByStoreId
    hash: 514ea112ed07
  - coordinate: PUT /api/brand-settings
    operationId: CatalogModuleBrandSetting_Update
    hash: f9ff2a7867df
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/brand-settings

2 operations under `/api/brand-settings`, served by module `VirtoCommerce.Catalog`, published under the tag "Catalog".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/brand-settings/store/{storeId}`<br>`CatalogModuleBrandSetting_GetByStoreId` | — | `storeId` (path) | `BrandStoreSetting` |
| `PUT /api/brand-settings`<br>`CatalogModuleBrandSetting_Update` | — | body application/json (optional) | 204 |

Module `VirtoCommerce.Catalog` — Catalog: Easily manage your products, categories, variations, and properties

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-brand-settings.json`.
