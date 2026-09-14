---
id: KB-288ECE2D
subject: rest-api-sitemaps
plane: derived-first
question: Which endpoints does this deployment serve under /api/sitemaps, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Sitemaps
    version: 3.1000.1
anchors:
  - coordinate: DELETE /api/sitemaps
    operationId: SitemapsModuleApi_DeleteSitemap
    hash: 15ff4c190b6d
  - coordinate: DELETE /api/sitemaps/items
    operationId: SitemapsModuleApi_RemoveSitemapItems
    hash: 4a25a01c507e
  - coordinate: GET /api/sitemaps/{id}
    operationId: SitemapsModuleApi_GetSitemapById
    hash: 9d3f5c4cc4a3
  - coordinate: GET /api/sitemaps/download
    operationId: SitemapsModuleApi_DownloadSitemap
    hash: 57872fd1f682
  - coordinate: GET /api/sitemaps/exportToStoreAssets
    operationId: SitemapsModuleApi_ExportToStoreAssets
    hash: 305b15dae204
  - coordinate: GET /api/sitemaps/generate
    operationId: SitemapsModuleApi_GenerateSitemap
    hash: 336b6e67d0d7
  - coordinate: GET /api/sitemaps/schema
    operationId: SitemapsModuleApi_GetSitemapsSchema
    hash: 2166da1ff3de
  - coordinate: POST /api/sitemaps
    operationId: SitemapsModuleApi_AddSitemap
    hash: 9197308e2936
  - coordinate: POST /api/sitemaps/{sitemapId}/items
    operationId: SitemapsModuleApi_AddSitemapItems
    hash: e68fbd1ceb25
  - coordinate: POST /api/sitemaps/items/search
    operationId: SitemapsModuleApi_SearchSitemapItems
    hash: 2b9770de6d6b
  - coordinate: POST /api/sitemaps/search
    operationId: SitemapsModuleApi_SearchSitemaps
    hash: 1aaaecf3a629
  - coordinate: PUT /api/sitemaps
    operationId: SitemapsModuleApi_UpdateSitemap
    hash: 348b62107064
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/sitemaps

12 operations under `/api/sitemaps`, served by module `VirtoCommerce.Sitemaps`, published under the tag "Sitemap Generator".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/sitemaps`<br>`SitemapsModuleApi_DeleteSitemap` | — | — | 204 |
| `DELETE /api/sitemaps/items`<br>`SitemapsModuleApi_RemoveSitemapItems` | — | — | 204 |
| `GET /api/sitemaps/{id}`<br>`SitemapsModuleApi_GetSitemapById` | — | `id` (path) | `Sitemap` |
| `GET /api/sitemaps/download`<br>`SitemapsModuleApi_DownloadSitemap` | — | — | `SitemapDownloadNotification` |
| `GET /api/sitemaps/exportToStoreAssets`<br>`SitemapsModuleApi_ExportToStoreAssets` | — | — | `SitemapDownloadNotification` |
| `GET /api/sitemaps/generate`<br>`SitemapsModuleApi_GenerateSitemap` | — | — | `string` |
| `GET /api/sitemaps/schema`<br>`SitemapsModuleApi_GetSitemapsSchema` | — | — | `string[]` |
| `POST /api/sitemaps`<br>`SitemapsModuleApi_AddSitemap` | — | body application/json (optional) | 204 |
| `POST /api/sitemaps/{sitemapId}/items`<br>`SitemapsModuleApi_AddSitemapItems` | — | `sitemapId` (path), body `SitemapItem[]` (optional) | 204 |
| `POST /api/sitemaps/items/search`<br>`SitemapsModuleApi_SearchSitemapItems` | — | body application/json (optional) | `SitemapItemsSearchResult` |
| `POST /api/sitemaps/search`<br>`SitemapsModuleApi_SearchSitemaps` | — | body application/json (optional) | `SitemapSearchResult` |
| `PUT /api/sitemaps`<br>`SitemapsModuleApi_UpdateSitemap` | — | body application/json (optional) | 204 |

Module `VirtoCommerce.Sitemaps` — Sitemap Generator: Simplifies the process of generating sitemaps for online stores

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-sitemaps.json`.
