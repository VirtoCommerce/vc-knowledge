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
  - method: observation + source
    deployment: vcptcore_dev
    at: 2026-10-05T22:31:38.203Z
    by: session:f9a8159e
---
On B2B-store, whose VirtoPages.Enable setting is false (the descriptor default), anonymous Query.pageDocuments listed the page-builder page "/qa-partner-portal-support", while Query.slugInfo(permalink: "qa-partner-portal-support", storeId: "B2B-store", cultureName: "en-US") returned entityInfo null and redirectUrl null; a storefront hard load showed the 404 page at HTTP 200 and added a hit to that permalink's broken link. The source explains it: PageDocumentSeoResolver.FindSeoAsync (vc-module-pages) returns no records unless the store's VirtoPages.Enable is true, while the XCMS pageDocuments query does not check that switch, and the storefront only queries pageDocuments when the switch is on. The resolver adds the leading slash itself, so the slash in the listed permalink is not the cause. "Listed but unresolved" is therefore config-gated rather than a resolver defect: turn VirtoPages.Enable on for the store to make such a page resolve.
