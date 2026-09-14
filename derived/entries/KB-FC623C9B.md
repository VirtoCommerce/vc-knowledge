---
id: KB-FC623C9B
subject: rest-api-catalog-listentrylinks
plane: derived-first
question: Which endpoints does this deployment serve under /api/catalog/listentrylinks, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Catalog
    version: 3.1002.9
anchors:
  - coordinate: POST /api/catalog/listentrylinks
    operationId: CatalogModuleListEntry_CreateLinks
    hash: 3c897f1ce1dd
  - coordinate: POST /api/catalog/listentrylinks/bulkcreate
    operationId: CatalogModuleListEntry_BulkCreateLinks
    hash: fc24f7c2fa12
  - coordinate: POST /api/catalog/listentrylinks/delete
    operationId: CatalogModuleListEntry_DeleteLinks
    hash: 515f07fa7597
  - coordinate: POST /api/catalog/listentrylinks/search
    operationId: CatalogModuleListEntry_SearchLinks
    hash: 5a857b3a9adb
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/catalog/listentrylinks

4 operations under `/api/catalog/listentrylinks`, served by module `VirtoCommerce.Catalog`, published under the tag "Catalog".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `POST /api/catalog/listentrylinks`<br>`CatalogModuleListEntry_CreateLinks` | Creates links for categories or items to parent categories and catalogs. | body `CategoryLink[]` (optional) | 200 |
| `POST /api/catalog/listentrylinks/bulkcreate`<br>`CatalogModuleListEntry_BulkCreateLinks` | Creates category links in bulk for catalog entries based on the specified search criteria. | body application/json (optional) | 200 |
| `POST /api/catalog/listentrylinks/delete`<br>`CatalogModuleListEntry_DeleteLinks` | Unlinks the linked categories or items from parent categories and catalogs. | body `CategoryLink[]` (optional) | 204 |
| `POST /api/catalog/listentrylinks/search`<br>`CatalogModuleListEntry_SearchLinks` | — | body application/json (optional) | 200 |

Module `VirtoCommerce.Catalog` — Catalog: Easily manage your products, categories, variations, and properties

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-catalog-listentrylinks.json`.
