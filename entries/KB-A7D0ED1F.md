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
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.XCatalog=3.1024.0-pr-115-8339; store=B2B-store; role=anonymous
    at: 2026-11-04T09:32:32.425Z
    by: session:322b37a0
    bodyHash: a76cd1498687
    who: Lenajava1
    note: "2026-11-04, other catalog = a physical catalog not linked into the store that owns two properties (one type Catalog, one type Product) whose names the store's answer does not contain: Query.properties(storeId, types [CATALOG, CATEGORY, PRODUCT, VARIATION], cultureName en-US) returned the same 19 store properties unfiltered and with filter catalogIds:<that catalog>; neither of the other catalog's properties appeared."
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.XCatalog=3.1024.0-pr-115-8339; store=B2B-store; role=storefront buyer
    at: 2026-11-04T09:43:34.984Z
    by: session:322b37a0
    bodyHash: a76cd1498687
    who: Lenajava1
    note: "2026-11-04, as an AcmeCorp buyer: filter catalogIds:<physical catalog outside the store, 2 properties read via admin GET /api/catalog/catalogs/{id}?responseGroup=Full> returned the same totalCount as unfiltered, and neither of that catalog's property names appeared in the unfiltered or the widened list."
---
On vcst_qa (XCatalog 3.1024.0-pr-115-8339), Query.properties(storeId B2B-store, types [CATALOG, CATEGORY, PRODUCT, VARIATION], cultureName) returned 19 properties, all with catalogId = the store's catalog (each resolved via GET /api/catalog/properties/{id}); with filter "catalogIds:<another catalog id>" it returned the same 19 store-catalog properties (3 runs). On pre-fix XCatalog 3.1023.0 (vcptcore) the same filter returned 14 properties, all from the other catalog.
