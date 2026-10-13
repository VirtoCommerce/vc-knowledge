---
id: KB-FA8E7A5F
subject: Sitemaps on-demand download delivers a ZIP archive (sitemap.xml index + sitemap/ folder) at notification.downloadUrl, not a bare XML file
plane: experiential
question: "GET /api/sitemaps/download notification downloadUrl: is the file a zip archive or sitemap XML?"
status: active
appliesTo:
  - axis: module
    value: sitemaps
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: GET /api/sitemaps/download
  - coordinate: POST /api/platform/pushnotifications
evidence:
  - method: observation + source
    deployment: vcptcore_regression
    at: 2026-10-13T15:46:21.123Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:40:34.689Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-observed ~18:11Z: finished in 7s; downloadUrl is absolute and carries a ?v=<ticks> cache-buster after .zip; entries sitemap.xml (sitemapindex root), sitemap/vendor.xml, sitemap/test.xml, sitemap/catalog.xml. Poll with POST /api/platform/pushnotifications {ids:[id]} — GET on that route is 405."
---
On Stable 16 (Sitemaps 3.1006.0 + BackgroundJobs 3.1052.0) GET /api/sitemaps/download?storeId=B2B-store&baseUrl=<storefront> returns 200 with a push-notification JSON; the notification finished within ~10s with downloadUrl /cms-content/assets/tmp/sitemap-<date>-<guid>.zip; fetching it returned 200 application/x-zip-compressed (PK header). Unzipped: sitemap.xml with a <sitemapindex> root plus a sitemap/ folder of child sitemaps. Source SitemapDownloadJobHandler: 'Packs a store's sitemap files into a downloadable archive'.
