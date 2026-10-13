---
id: KB-688E2CBC
subject: PATCH /api/catalog/categories/{id} isActive writes exactly one CatalogProduct Modified change-log entry per child, whether or not Catalog.Search.EventBasedIndexation.Enable is on; only the search reflection depends on the flag
plane: experiential
question: Does a category visibility PATCH re-save its child products, and does that depend on the Catalog event-indexation flag?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: PATCH /api/catalog/categories/{id}
  - coordinate: POST /api/platform/changelog/search
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:40:35.143Z
    by: session:624d35a1
    who: kutasinaelena
---
vcptcore_regression, Catalog 3.1048.0, 2026-10-13. Four PATCH /api/catalog/categories/{Ladders} [{op:replace,path:/isActive,value:...}] calls (all 204): two with Catalog.Search.EventBasedIndexation.Enable=false, two with it true. Each produced one Category Modified entry (detail VisibilityChange) and ~0.1-0.15 s later exactly one CatalogProduct Modified entry for the single child product. POST /api/platform/changelog/search {objectType:"CatalogProduct", objectIds:[id]} returns a BARE JSON array (no totalCount/results wrapper). Search reflection differed: flag false -> the child stayed in anonymous xAPI products for 120 s after deactivation; flag true -> gone in 0.3 s and back 6 s after the revert.
