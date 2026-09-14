---
id: KB-EF433E0E
subject: rest-api-catalog-measures
plane: derived-first
question: Which endpoints does this deployment serve under /api/catalog/measures, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Catalog
    version: 3.1002.9
anchors:
  - coordinate: DELETE /api/catalog/measures
    operationId: CatalogModuleMeasures_DeleteMeasures
    hash: 8e42059cde63
  - coordinate: GET /api/catalog/measures/{id}
    operationId: CatalogModuleMeasures_GetMeasureById
    hash: 038fc54ed1e1
  - coordinate: GET /api/catalog/measures/default
    operationId: CatalogModuleMeasures_GetDefaultMeasures
    hash: 33266eb38c9e
  - coordinate: PATCH /api/catalog/measures/{id}
    operationId: CatalogModuleMeasures_PatchMeasure
    hash: ef5994ca9fb0
  - coordinate: POST /api/catalog/measures
    operationId: CatalogModuleMeasures_CreateMeasure
    hash: 31a7df408db2
  - coordinate: POST /api/catalog/measures/search
    operationId: CatalogModuleMeasures_SearchMeasures
    hash: 27f9166c1280
  - coordinate: PUT /api/catalog/measures
    operationId: CatalogModuleMeasures_UpdateMeasure
    hash: d33dfd305612
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/catalog/measures

7 operations under `/api/catalog/measures`, served by module `VirtoCommerce.Catalog`, published under the tag "Catalog".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/catalog/measures`<br>`CatalogModuleMeasures_DeleteMeasures` | — | — | 204 |
| `GET /api/catalog/measures/{id}`<br>`CatalogModuleMeasures_GetMeasureById` | — | `id` (path) | `Measure` |
| `GET /api/catalog/measures/default`<br>`CatalogModuleMeasures_GetDefaultMeasures` | — | — | `Measure[]` |
| `PATCH /api/catalog/measures/{id}`<br>`CatalogModuleMeasures_PatchMeasure` | Partial update for the specified Measure by id | `id` (path), body `Operation[]` (optional) | 204 |
| `POST /api/catalog/measures`<br>`CatalogModuleMeasures_CreateMeasure` | — | body `Measure[]` (optional) | `Measure[]` |
| `POST /api/catalog/measures/search`<br>`CatalogModuleMeasures_SearchMeasures` | — | body application/json (optional) | `MeasureSearchResult` |
| `PUT /api/catalog/measures`<br>`CatalogModuleMeasures_UpdateMeasure` | — | body application/json (optional) | 204 |

Module `VirtoCommerce.Catalog` — Catalog: Easily manage your products, categories, variations, and properties

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-catalog-measures.json`.
