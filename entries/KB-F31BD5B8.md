---
id: KB-F31BD5B8
subject: The xAPI products filter covers products only by default; variations need an explicit is:variation or is:product,variation scope
plane: experiential
question: Are variations included when Query.products is filtered by code, gtin or manufacturerPartNumber?
questions:
  - text: Why can't I find a specific size or colour variant by its item code?
  - text: Does filtering the catalog by manufacturer part number also return variants of a master product?
  - text: "Which is: scope must be added to a Query.products filter to match variations?"
  - text: When a manufacturer part number exists only on a variation, does the parent product match the filter?
concepts:
  - id: product-variation
  - id: product-filter
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T13:30:12.406Z
    by: session:p44504
    who: Lenajava1
    splitFrom: KB-A0B3940F
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T13:30:12.971Z
    by: session:p51828
    who: Lenajava1
    splitFrom: KB-A0B3940F
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T14:33:01.019Z
    by: session:p30132
    who: Lenajava1
    note: "Re-observed 2026-09-28 during the barcode seed proof: code:\"<sku>\" matched exactly one product and the strict prefix code:\"QaBc-2945\" matched 0. The two variations resolved only with is:variation, and an MPN held only by a variation needed is:product,variation."
    splitFrom: KB-A0B3940F
  - method: observation
    deployment: vcst
    at: 2026-09-28T14:12:30.336Z
    by: session:p16868
    who: Lenajava1
    contradicts: true
    note: "A quoted term is not a pure exact match: * and ? inside the quotes are honoured as wildcards. gtin:\"2945100000*\" -> 1 hit, gtin:\"*\" -> 318, code:\"QA-BC-2945-00?\" -> 8 (Query.products, storeId AGENT-TEST store on B2B-mixed, Elasticsearch8). A strict prefix WITHOUT a wildcard does return 0, as the entry says."
    splitFrom: KB-A0B3940F
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:21:48.490Z
    by: session:p52508
    who: Lenajava1
    contradicts: true
    note: Through the storefront barcode expansion, the quoted term gtin:"2945100000*" (and the same term on code, manufacturerPartNumber and a short-text property) matched 1 product, so * inside the quotes is honoured as a wildcard and the quoted term is not a pure whole-value match
    splitFrom: KB-A0B3940F
---
In xAPI products(storeId, filter) the default scope is products only: a variation is found with is:variation code:"...", and a manufacturerPartNumber held only by a variation is found with is:product,variation manufacturerPartNumber:"..." (1 hit; the parent does not match).
