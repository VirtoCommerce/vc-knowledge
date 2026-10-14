---
id: KB-7E41EAB9
subject: After deleting a category, GET /api/catalog/categories/{id} keeps returning 200 for descendant categories that were cascade-deleted, if they were in the platform cache
plane: experiential
question: Does GET /api/catalog/categories/{id} return 404 for subcategories removed by deleting their parent category?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: operation
    value: category-delete-cascade
  - axis: surface
    value: admin-api
anchors:
  - coordinate: DELETE /api/catalog/categories
  - coordinate: GET /api/catalog/categories/{id}
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T06:19:58.238Z
    by: session:624d35a1
    who: kutasinaelena
---
2026-10-14, Catalog 3.1048.0. Tree P > C > G with a product in G; C had been loaded (G created under it). DELETE /api/catalog/categories?ids=P -> 204. GET P 404, GET G 404, product 404, but GET C stays 200 (parentId = deleted P) for >12 min and for every respGroup. The DB row is gone: POST /api/catalog/categories with parentId=C fails 500 FK_Category_Category_ParentCategoryId. Same after an Admin UI delete (Confirm delete with 'yes'). Read deletes of descendants through cache-backed GET as unreliable; probe with a write or wait for cache expiry.
