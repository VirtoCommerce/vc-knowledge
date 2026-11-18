---
id: KB-0DB2D9E3
subject: Repeating the same storefront keyword search returns the same products in the same order (SearchProducts and GetSearchResults agree)
plane: experiential
question: Is the result order of a storefront keyword search stable when the same search is repeated?
questions:
  - text: Does /search?q= relevance ordering change between identical searches?
  - text: Do SearchProducts and GetSearchResults return the same order for one keyword?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /search
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcst_qa
    build: f75b254d87d474d1
    conditions: module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9; module:VirtoCommerce.Xapi=3.1033.0-pr-79-4dfa; theme=2.63.0-pr-2666; store=B2B-store
    at: 2026-11-18T10:28:17.736Z
    by: session:25a30bd6
    who: Lenajava1
---
Signed-in B2B user, store B2B-store. The keyword "epson" was searched 3 times over ~6 minutes (once by URL /search?q=epson, twice typed in the header box after removing the category scope). Each time SearchProducts and GetSearchResults both returned totalCount 8 with the same 8 product codes in the same order, and the rendered result cards (name, price, stock) were identical. Same for "shirt" (4 results) and for the /printers category page (totalCount 20, first 16 cards, default sort). No GraphQL errors[] in any response.
