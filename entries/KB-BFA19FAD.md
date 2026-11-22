---
id: KB-BFA19FAD
subject: A ProductComparison.MaxItems value saved in the Admin store Settings blade is returned by the storefront's GetPageContext (pageContext.store.settings.modules) and InitializeApplication on the next hard reload, with no deploy or cache reset
plane: experiential
question: Does a ProductComparison store setting saved in the Admin reach the storefront's GetPageContext pageContext.store.settings.modules after a reload?
questions:
  - text: Which storefront GraphQL operations carry the ProductComparison store settings?
  - text: Is ProductComparison.MaxItems a public store setting visible to xAPI?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.pageContext
  - coordinate: Query.store
  - coordinate: PUT /api/stores
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 6cf3e54e8335fd8d
    conditions: module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-7403; store=B2B-store
    at: 2026-11-22T23:08:49.281Z
    by: session:fcdb91ae
    who: kutasinaelena
---
Admin > Configuration > Stores > B2B-store > Settings widget > Product Comparison group: changed 'Products per category' 5 -> 3 and saved (PUT /api/stores 204; GET /api/stores/{id} then returned ProductComparison.MaxItems=3, isPublic true). Within ~20 s, a hard reload of the storefront /compare issued InitializeApplication (store.settings.modules) and GetPageContext (pageContext.store.settings.modules); both answered errors [] with module VirtoCommerce.ProductComparison listing Enabled=true, MaxItems=3, AnonymousListEnabled=true. No cache reset was needed. Restored to 5 afterwards.
