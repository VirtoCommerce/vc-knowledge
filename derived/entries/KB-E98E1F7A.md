---
id: KB-E98E1F7A
subject: rest-api-catalog-videos
plane: derived-first
question: Which endpoints does this deployment serve under /api/catalog/videos, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Catalog
    version: 3.1002.9
anchors:
  - coordinate: DELETE /api/catalog/videos
    operationId: CatalogModuleVideos_Delete
    hash: 69580e2ed43a
  - coordinate: GET /api/catalog/videos/options
    operationId: CatalogModuleVideos_GetOptions
    hash: a9cfea561b2e
  - coordinate: PATCH /api/catalog/videos/{id}
    operationId: CatalogModuleVideos_PatchVideo
    hash: a7e3ab255140
  - coordinate: POST /api/catalog/videos
    operationId: CatalogModuleVideos_Update
    hash: b2243fc1fc19
  - coordinate: POST /api/catalog/videos/create
    operationId: CatalogModuleVideos_CreateVideo
    hash: ba474df138eb
  - coordinate: POST /api/catalog/videos/search
    operationId: CatalogModuleVideos_SearchVideos
    hash: 99ff802d1eaf
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/catalog/videos

6 operations under `/api/catalog/videos`, served by module `VirtoCommerce.Catalog`, published under the tag "Catalog".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/catalog/videos`<br>`CatalogModuleVideos_Delete` | Delete videos by ids | — | 204 |
| `GET /api/catalog/videos/options`<br>`CatalogModuleVideos_GetOptions` | Get video options from configuration | — | `VideoOptions` |
| `PATCH /api/catalog/videos/{id}`<br>`CatalogModuleVideos_PatchVideo` | Partial update for the specified Video by id | `id` (path), body `Operation[]` (optional) | 204 |
| `POST /api/catalog/videos`<br>`CatalogModuleVideos_Update` | Create new or update existing videos | body `Video[]` (optional) | `Video[]` |
| `POST /api/catalog/videos/create`<br>`CatalogModuleVideos_CreateVideo` | Create video | body application/json (optional) | `Video` |
| `POST /api/catalog/videos/search`<br>`CatalogModuleVideos_SearchVideos` | Search videos | body application/json (optional) | `VideoSearchResult` |

Module `VirtoCommerce.Catalog` — Catalog: Easily manage your products, categories, variations, and properties

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-catalog-videos.json`.
