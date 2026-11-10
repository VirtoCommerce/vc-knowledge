---
id: KB-68A6AACA
subject: On Catalog 3.1052.0-pr-916, deleting a catalog also deletes its products that sit under another catalog's category (no FK 500); DELETE /api/catalog/catalogs?ids= is 405, the id goes in the path
plane: experiential
question: What does DELETE /api/catalog/catalogs/{id} do to products whose catalogId is the deleted catalog but whose categoryId belongs to another catalog?
questions:
  - text: Does deleting a catalog still fail with FK_Item_Catalog_CatalogId when it holds products in a foreign category?
  - text: "Which URL form deletes a catalog through REST: ?ids= or the id in the path?"
  - text: Does deleting a catalog remove products of another catalog that are linked into it?
  - text: Does a catalog delete remove the links of its products in a virtual catalog?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: surface
    value: admin-spa
  - axis: surface
    value: rest-api
anchors:
  - coordinate: DELETE /api/catalog/catalogs/{id}
  - coordinate: DELETE /api/catalog/catalogs
  - coordinate: POST /api/catalog/products
  - coordinate: POST /api/catalog/listentries
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 4406dce8feff49f9
    conditions: platform=3.1095.0; module:VirtoCommerce.Catalog=3.1052.0-pr-916-eefd; module:VirtoCommerce.CatalogCsvImportModule=3.1009.0
    at: 2026-11-10T19:00:12.894Z
    by: session:9ad86dac
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_stable
    build: 09befca80f31cafd
    conditions: module:VirtoCommerce.Catalog=3.1029.9; module:VirtoCommerce.Core=3.1007.1
    at: 2026-11-10T22:25:49.272Z
    by: session:9ad86dac
    bodyHash: b1c5806d6cd9
    who: kutasinaelena
    note: "Same result on the support-line hotfix Catalog 3.1029.9, over REST only: DELETE /api/catalog/catalogs/{B} returned 204 on 3 fresh runs plus the extended run; B, its foreign-category product P, root product Q, own-category product R, variation V and category CB all return 404 afterwards; A, CA, A's product S survive; product L owned by A and linked into CB survives with catalogId A and links emptied. A plain catalog with its own category and 2 products also deletes with 204 and all of it returns 404."
---
vcptcore_qa1, Catalog 3.1052.0-pr-916-eefd, platform 3.1095.0. Built over REST: catalog A with category CA, catalog B, and product P created by POST /api/catalog/products with catalogId=B and categoryId=CA (200, no validation; GET returns both values as sent). Admin SPA Catalog list, Delete from B's context menu, typed yes, Confirm: the SPA sends DELETE /api/catalog/catalogs/{B}, which returned 204, three times on fresh data, with no 500. Afterwards GET catalog B and GET product P return 404; listentries for CA no longer lists P; A, CA and A's own product S survive. In one run B also held Q (B root, no category), R (in B's own category CB), and a variation V of P with catalogId B. All of them and CB returned 404 after the delete. Product L owned by A and linked into CB via POST /api/catalog/listentrylinks survived with catalogId A and its links emptied. B products linked into a virtual catalog's category: B deleted with 204, the virtual catalog and its category survive, and its listentries and listentrylinks/search are empty. A catalog with 30 foreign-category products returned 204 in 2.4 s and all 30 products are 404. DELETE /api/catalog/catalogs?ids={id} returned 405 and deleted nothing, so the id goes in the path. Before the fix this delete returned 500 FK_Item_Catalog_CatalogId (KB-B8D4B01A, VCST-6665).
