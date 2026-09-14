---
id: KB-314BAA6F
subject: rest-api-catalog-search
plane: derived-first
question: Which endpoints does this deployment serve under /api/catalog/search, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Catalog
    version: 3.1002.9
anchors:
  - coordinate: POST /api/catalog/search/categories
    operationId: CatalogModuleIndexedSearch_SearchCategories
    hash: 49e12ce04366
  - coordinate: POST /api/catalog/search/products
    operationId: CatalogModuleIndexedSearch_SearchProducts
    hash: 0eb66136f0a7
  - coordinate: POST /api/catalog/search/products/suggestions
    operationId: CatalogModuleIndexedSearch_GetProductSuggestions
    hash: b34f3be312d6
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/catalog/search

3 operations under `/api/catalog/search`, served by module `VirtoCommerce.Catalog`, published under the tag "Catalog".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `POST /api/catalog/search/categories`<br>`CatalogModuleIndexedSearch_SearchCategories` | — | body application/json (optional) | `CategoryIndexedSearchResult` |
| `POST /api/catalog/search/products`<br>`CatalogModuleIndexedSearch_SearchProducts` | — | body application/json (optional) | `ProductIndexedSearchResult` |
| `POST /api/catalog/search/products/suggestions`<br>`CatalogModuleIndexedSearch_GetProductSuggestions` | — | — | `SuggestionResponse` |

Module `VirtoCommerce.Catalog` — Catalog: Easily manage your products, categories, variations, and properties

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-catalog-search.json`.
