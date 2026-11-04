---
id: KB-A7D0ED1F
subject: Query.properties ignores a catalogIds filter term on the VCST-6251 fixed build and returns only the store catalog's properties
plane: experiential
question: Can Query.properties be made to return another catalog's properties with filter "catalogIds:<id>"?
questions:
  - text: Does the properties query return only the store catalog's properties?
  - text: Does the xAPI properties filter override the store catalog?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.properties
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.XCatalog=3.1024.0-pr-115-8339; store=B2B-store
    at: 2026-11-04T08:15:12.833Z
    by: session:7c4c844c
    who: Lenajava1
---
On vcst_qa (XCatalog 3.1024.0-pr-115-8339), Query.properties(storeId B2B-store, types [CATALOG, CATEGORY, PRODUCT, VARIATION], cultureName) returned 19 properties, all with catalogId = the store's catalog (each resolved via GET /api/catalog/properties/{id}); with filter "catalogIds:<another catalog id>" it returned the same 19 store-catalog properties (3 runs). On pre-fix XCatalog 3.1023.0 (vcptcore) the same filter returned 14 properties, all from the other catalog.
