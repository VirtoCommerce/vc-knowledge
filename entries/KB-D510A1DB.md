---
id: KB-D510A1DB
subject: Aggregation properties for store facets are read only at GET /api/catalog/aggregationproperties/{storeId}/properties; the bare GET /api/catalog/aggregationproperties is 404
plane: experiential
question: Which REST endpoint lists a store's facet (aggregation) properties with isSelected?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/catalog/aggregationproperties/{storeId}/properties
  - coordinate: PUT /api/catalog/aggregationproperties/{storeId}/properties
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T19:34:57.112Z
    by: session:624d35a1
    who: kutasinaelena
---
Catalog 3.1048.0, 2026-10-13: GET /api/catalog/aggregationproperties/B2B-store/properties returns 200 with the attribute/price-range rows and isSelected per row (53 rows); GET /api/catalog/aggregationproperties (no store segment) returns 404. The Admin Facets blade saves through PUT /api/catalog/aggregationproperties/{storeId}/properties (204), which rewrites the store setting Catalog.BrowseFilters.FilteredBrowsing.
