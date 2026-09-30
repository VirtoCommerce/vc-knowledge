---
id: KB-3BFF73A3
subject: "xAPI products filter: sku: is accepted as an alias of code, but mpn: is not an alias of manufacturerPartNumber and matches nothing"
plane: experiential
question: Which field names does Query.products filter accept for a product's SKU and manufacturer part number?
questions:
  - text: Can I search the shop by manufacturer part number using a short field name?
  - text: Which filter field names work for SKU and part number when filtering the product catalog?
  - text: Is sku an alias of code in the products query filter, and is mpn an alias of manufacturerPartNumber?
  - text: What does the products filter return for a field name that is not in the search index?
concepts:
  - id: product-filter
  - id: product-identifier
status: active
appliesTo:
  - axis: element
    value: products-filter-field-names
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:22:50.711Z
    by: session:p46996
    who: Lenajava1
---
Anonymous products(storeId: B2B-store, filter) on vcst, XCatalog 3.1022 PR 113 build: filter sku:"<code>" returned the product and the response reported the filter under the name code; code:"<code>" returned the same product. filter mpn:"<mpn>" (with is:product,variation) returned 0 and echoed a filter named mpn, while manufacturerPartNumber:"<mpn>" returned the variation. A filter on a field name the index does not have returns 0 with no errors[] and echoes the name as a filter. The barcode scanner field list offers code, gtin and manufacturerPartNumber, not sku or mpn.
