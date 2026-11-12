---
id: KB-48CBEB82
subject: Query.slugInfo resolves no Virto Page while the store setting VirtoPages.Enable is false, although Query.pageDocuments still lists the page
plane: experiential
question: Why does a Virto Page that pageDocuments lists return entityInfo null from slugInfo?
status: active
appliesTo:
  - axis: module
    value: pages
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.pageDocuments
  - coordinate: store setting VirtoPages.Enable
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T22:31:38.203Z
    by: session:f9a8159e
    note: "how: observation + source"
    build: 8db313999cee1432
    buildFrom: deploy-history
  - method: observation
    deployment: local
    build: 7cdc59b82cc67198
    conditions: module:VirtoCommerce.Pages=3.1012.0; module:VirtoCommerce.XCMS=3.1009.0; module:VirtoCommerce.PageBuilderModule=3.1041.0-pr-182-2743; store=B2B-store
    at: 2026-11-12T12:37:58.533Z
    by: session:57071c91
    bodyHash: 3dc3a4ba9c62
    who: Lenajava1
    note: "Re-observed on a fresh local stack with the switch flipped both ways: 7 published page-builder pages (Published/Public/in-window in the pages index) all returned slugInfo entityInfo null with VirtoPages.Enable=false; after PUT /api/stores set it true, every public page resolved as objectType Pages, isActive true, with no other change. An org-restricted page still returned null for an anonymous query. The repo's store seed does not set this switch, so a freshly seeded store has it off."
---
On B2B-store, whose VirtoPages.Enable setting is false (the descriptor default), anonymous Query.pageDocuments listed the page-builder page "/qa-partner-portal-support", while Query.slugInfo(permalink: "qa-partner-portal-support", storeId: "B2B-store", cultureName: "en-US") returned entityInfo null and redirectUrl null; a storefront hard load showed the 404 page at HTTP 200 and added a hit to that permalink's broken link. The source explains it: PageDocumentSeoResolver.FindSeoAsync (vc-module-pages) returns no records unless the store's VirtoPages.Enable is true, while the XCMS pageDocuments query does not check that switch, and the storefront only queries pageDocuments when the switch is on. The resolver adds the leading slash itself, so the slash in the listed permalink is not the cause. "Listed but unresolved" is therefore config-gated rather than a resolver defect: turn VirtoPages.Enable on for the store to make such a page resolve.
