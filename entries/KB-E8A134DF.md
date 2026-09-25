---
id: KB-E8A134DF
subject: the storefront robots.txt is store content uploaded in Admin, not frontend code
plane: experiential
question: Where does the storefront's /robots.txt come from, and how is it changed?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: /robots.txt
  - coordinate: /sitemap/sitemap.xml
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:18:41.544Z
    by: session:memimpor
    who: Lenajava1
---
The storefront robots.txt is store content, not part of the frontend code, and changing it needs no redeploy: in Admin, open the store, use its Assets widget and upload a custom robots.txt, which overrides the system-generated one; separate policies per domain mean separate stores. Editing robots.txt inside a theme through the Content module is the legacy storefront pattern and does not apply to the current Vue storefront. On this deployment the sitemap is served at /sitemap/sitemap.xml and the root /sitemap.xml returns 404. robots.txt crawl control is independent of the per-route robots meta tag.
