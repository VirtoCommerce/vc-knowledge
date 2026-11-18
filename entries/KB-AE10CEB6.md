---
id: KB-AE10CEB6
subject: With Catalog.Search.UseFullObjectIndexStoring=false, product index documents carry no __object and xAPI loads each product from the database; only __variations, __minvariationprice and __score are bound from the index
plane: experiential
question: Does xAPI Query.products / Query.product read the product from the index __object or from the database on a stand where Store serialized catalog objects in index is off?
questions:
  - text: Is the xCatalog CatalogProductBinder (__object) exercised on vcst?
  - text: Why does the Product index document have no __object field?
  - text: Which index fields does xAPI bind for a product when full-object storing is disabled?
status: active
appliesTo:
  - axis: layer
    value: search-index
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
  - coordinate: Query.product
  - coordinate: GET /api/search/indexes/index/Product/{id}
  - coordinate: GET /api/platform/settings/Catalog.Search.UseFullObjectIndexStoring
evidence:
  - method: observation
    deployment: vcst_qa
    build: f75b254d87d474d1
    conditions: module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9; module:VirtoCommerce.Xapi=3.1033.0-pr-79-4dfa; module:VirtoCommerce.Catalog=3.1052.0; module:VirtoCommerce.ElasticSearch8=3.1012.0; setting:Catalog.Search.UseFullObjectIndexStoring=false; setting:Catalog.Search.EventBasedIndexation.Enable=false
    at: 2026-11-18T10:27:34.530Z
    by: session:25a30bd6
    who: Lenajava1
---
On vcst_qa (B2B-store), GET /api/platform/settings/Catalog.Search.UseFullObjectIndexStoring returned value false (default false). GET /api/search/indexes/index/Product/{id} for three products returned documents with __content, __variations, __minvariationprice (JSON strings like {"currency":"USD","value":285.0}), __outline, __score and plain fields, but no __object. Query.product / Query.products still returned complete products whose name, code, images, properties and variation ids equal admin REST GET /api/catalog/products/{id}?respGroup=Full: xCatalog's EnsureCatalogProductLoadedMiddleware loads every product whose IndexedProduct is null from the database. Consequence: on this stand the CatalogProductBinder __object deserialization path (and its JSON error guard) is never exercised; a fix to that path cannot be verified here without enabling the setting and rebuilding the index. The xAPI getting-started doc tells operators to enable this option and rebuild the index.
