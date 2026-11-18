---
id: KB-F9CBFB60
subject: A category linked at two places in the store's virtual catalog gets its outline and slug picked non-deterministically per call in Query.childCategories and Query.category
plane: experiential
question: Which outline and slug does xAPI return for a category that has two outlines in the store catalog, and is it stable between calls?
questions:
  - text: Why does a category slug flip between snacks/nachos and drinks-and-food/snacks/nachos?
  - text: Is childCategories output deterministic across identical calls?
status: active
appliesTo:
  - axis: entity
    value: category
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.childCategories
  - coordinate: Query.category
  - coordinate: GET /api/catalog/categories/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: f75b254d87d474d1
    conditions: module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9; module:VirtoCommerce.Xapi=3.1033.0-pr-79-4dfa; store=B2B-store; culture=en-US
    at: 2026-11-18T10:27:34.721Z
    by: session:25a30bd6
    who: Lenajava1
---
On B2B-store, 6 of 257 categories returned by Query.childCategories(maxLevel 2, onlyActive) have two outlines in the store's virtual catalog (e.g. nachos: catalog/drinks-and-food/snacks/nachos and catalog/snacks/nachos per GET /api/catalog/categories/{id} outlines). Over 8 identical calls (buyer and anonymous alternating) each such category came back with one outline/slug or the other, 4/4, uncorrelated with auth; Query.category(id) without previousOutline flips the same way. Everything else in the response was byte-identical between calls. Not shown whether this predates XCatalog 3.1027.0-pr-107 / Xapi 3.1033.0-pr-79 (no pre-build comparison).
