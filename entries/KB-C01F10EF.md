---
id: KB-C01F10EF
subject: A product card's "From" price on the storefront equals Query.products minVariationPrice, which is the lowest price among the product and its variations
plane: experiential
question: Where does the "From" price on a storefront card for a product with variations come from, and which variation does it match?
questions:
  - text: Does minVariationPrice include the master product's own price?
  - text: Is the From price on a variation product card the cheapest variation?
  - text: What does Query.products minVariationPrice return for a product with variations?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
  - coordinate: /search
evidence:
  - method: observation
    deployment: vcst_qa
    build: f75b254d87d474d1
    conditions: module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9; module:VirtoCommerce.Xapi=3.1033.0-pr-79-4dfa; theme=2.63.0-pr-2666; store=B2B-store
    at: 2026-11-18T10:28:17.641Z
    by: session:25a30bd6
    who: Lenajava1
---
Signed-in B2B user, store B2B-store, USD. /search?q=shirt card for a product with variations (XKC-38084072, own price $9.00, 5 variations + master = "6 variations" link) shows "From $8.00". SearchProducts returned price $9.00 and minVariationPrice $8.00 for that item. The PDP's variation query (SearchProducts filter productfamilyid:<id> is:product,variation) listed 6 items priced $9, $10, $8, $10, $8, $11 (the master is included). Picking either $8 variant in the PDP option picker showed $8.00 (one as list $12.00 / sale $8.00), a non-minimum one showed $11.00. So the card's From price is the minimum actual price across the master and its variations. Identical in 3 consecutive passes.
