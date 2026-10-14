---
id: KB-2CFCFAF8
subject: GET taggeditem for an untagged product returns a template with entityType "CatalogProduct"; PUTting it back creates an unusable row and later GETs for that entity 500 with message "CatalogProduct"
plane: experiential
question: Can I PUT back the template returned by GET /api/personalization/taggeditem/{id} for a product that has no tagged-item row?
status: active
appliesTo:
  - axis: module
    value: catalogpersonalization@3.1006.1
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/personalization/taggeditem/{id}
  - coordinate: PUT /api/personalization/taggeditem
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T06:28:45.449Z
    by: session:624d35a1
    who: kutasinaelena
---
On CatalogPersonalization 3.1006.1, GET /api/personalization/taggeditem/{productId} for a product without a row returns a synthesized item with label null, id null and entityType "CatalogProduct" (taken from the CLR type name). If you PUT it back unchanged, label null fails with a SQL NOT NULL error. With a label added, the row IS saved under entityType "CatalogProduct", but the outline sync then throws, so the PUT returns 500 {"message":"CatalogProduct"}. After that, GET for that entity 500s whenever FirstOrDefault picks the stray row. There is no DELETE endpoint; the only recovery is to re-point the row's entityId. The persisted, supported types are "Product" and "Category". The Admin UI (tag-list.js) sets entityType from blade.item.type, so the storefront and admin are not affected. API clients must send entityType Product or Category explicitly.
