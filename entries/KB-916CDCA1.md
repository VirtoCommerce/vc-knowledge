---
id: KB-916CDCA1
subject: A quoted xAPI products filter term matches the whole value case-insensitively but honours * and ? wildcards inside the quotes
plane: experiential
question: Does a quoted term in the Query.products filter match exactly, by prefix, or with wildcards?
questions:
  - text: If I search by the first part of a product code, will the catalog find the product?
  - text: Can a QA filter on a partial SKU or GTIN, or does the quoted value have to be complete?
  - text: Are asterisk and question mark treated as wildcards inside a quoted products filter value?
  - text: Is the Query.products field filter case-sensitive, and how do I escape a double quote in the value?
concepts:
  - id: product-filter
  - id: product-identifier
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
    resolved: claim-amended
    resolvedAt: 2026-10-08T11:34:58.011Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/catalog-xapi-20261008
    resolution: "The body states it: * and ? inside quotes are honoured, a strict prefix without a wildcard returns 0; re-observed live 2026-10-08 on three stands."
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:21:48.490Z
    by: session:p52508
    who: Lenajava1
    contradicts: true
    note: Through the storefront barcode expansion, the quoted term gtin:"2945100000*" (and the same term on code, manufacturerPartNumber and a short-text property) matched 1 product, so * inside the quotes is honoured as a wildcard and the quoted term is not a pure whole-value match
    splitFrom: KB-A0B3940F
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.Catalog=3.1048.0; module:VirtoCommerce.XCatalog=3.1023.0; module:VirtoCommerce.Inventory=3.1010.0; module:VirtoCommerce.Seo=3.1005.0; module:VirtoCommerce.ElasticSearch8=3.1011.0; store=B2B-store; setting:Catalog.Search.BarcodeSearchFields=[]
    at: 2026-10-08T11:34:43.943Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08, fresh products: code:\"<exact>\" 1; strict prefix without wildcard 0 (code and gtin); code:\"<prefix>*\" honoured; ? inside quotes honoured; lower- and upper-cased values match; an escaped double quote matches. Default scope is products only: a variation code/gtin returns 0 without is:variation and 1 with it; an mpn held only by a variation is found with is:product,variation."
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; module:VirtoCommerce.Catalog=3.1049.0-pr-910-04a5; module:VirtoCommerce.XCatalog=3.1023.0; module:VirtoCommerce.Inventory=3.1010.0; module:VirtoCommerce.Seo=3.1006.0-pr-22-7ef9; module:VirtoCommerce.ElasticSearch8=3.1011.0; store=B2B-store
    at: 2026-10-08T11:34:45.715Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08, fresh products: code:\"<exact>\" 1; strict prefix without wildcard 0 (code and gtin); code:\"<prefix>*\" honoured; ? inside quotes honoured; lower- and upper-cased values match; an escaped double quote matches. Default scope is products only: a variation code/gtin returns 0 without is:variation and 1 with it; an mpn held only by a variation is found with is:product,variation."
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.Catalog=3.1029.6; module:VirtoCommerce.XCatalog=3.1007.4; module:VirtoCommerce.Inventory=3.1003.0; module:VirtoCommerce.Seo=3.1004.0; module:VirtoCommerce.ElasticSearch8=3.1007.0; store=B2B-store
    at: 2026-10-08T11:34:47.344Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08, fresh products: code:\"<exact>\" 1; strict prefix without wildcard 0 (code and gtin); code:\"<prefix>*\" honoured; ? inside quotes honoured; lower- and upper-cased values match; an escaped double quote matches. Default scope is products only: a variation code/gtin returns 0 without is:variation and 1 with it; an mpn held only by a variation is found with is:product,variation."
---
In xAPI products(storeId, filter), a quoted term such as code:"ABC-1" or gtin:"<13 digits>" matches the whole value: a strict prefix of an existing product code, without a wildcard, returns 0. A quoted term is not a pure exact match, though: * and ? inside the quotes are honoured as wildcards - gtin:"2945100000*" returned 1 hit, gtin:"*" returned 318 and code:"QA-BC-2945-00?" returned 8 on an Elasticsearch 8 provider, and the same held through the storefront barcode expansion on code, gtin, manufacturerPartNumber and a short-text property. Values are matched case-insensitively on this deployment's search provider. A double quote inside a quoted value can be escaped with a backslash and still matches.
