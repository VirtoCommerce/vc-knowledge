---
id: KB-EC3BEB66
subject: xAPI categories returns 0 items when the filter carries product-only terms (price.USD:(0 TO), inStock_variations:true); the storefront sends categories a separate subtree-only filter
plane: experiential
question: Why does Query.categories return nothing when the filter includes price or inStock_variations terms?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.xcatalog
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.categories
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T19:08:49.253Z
    by: session:624d35a1
    who: kutasinaelena
---
On vcptcore_regression (XCatalog 3.1023.0) and vcptcore_stable (XCatalog 3.1007.4), categories(storeId:"B2B-store", currencyCode:"USD", filter:"category.subtree:<store catalog>") returns categories (116 and 401 without a query), but adding price.USD:(0 TO), or inStock_variations:true, or both, returns totalCount 0 for every query, while products with the same filter still returns items. VCST-4374 (search drop-down showed no categories) was fixed in the storefront, vc-frontend PR #2135 'correct filters for categories', which added a separate $categoriesFilter variable holding only the category.subtree expression. xAPI was not changed. A test that expects categories to survive product-only filter terms is asserting behaviour that was never shipped.
