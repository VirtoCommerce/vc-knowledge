---
id: KB-2E8CAC3D
subject: A slugInfo/pageContext request for a permalink no SEO resolver returns creates a broken-link record, and every later request for it bumps its HitCount
plane: experiential
question: Which requests create or bump broken-link records, and is it synchronous?
status: active
appliesTo:
  - axis: build
    value: pr-vcst-5989
  - axis: module
    value: virtocommerce.seo
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.pageContext
  - coordinate: POST /api/seo/broken-links/search
  - coordinate: POST /api/seoinfos/search
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T20:10:43.828Z
    by: session:f9a8159e
---
On vcptcore_dev (Seo 3.1006.0 PR build), an anonymous xAPI slugInfo(permalink, storeId, cultureName) for an existing Virto Page permalink that returned entityInfo null made the store's broken-link count go 70 to 71; the new record (status Active, HitCount 1, createdBy http:anonymous) was written about 150 ms after the request, i.e. through the background job, not the request thread. Hard-loading the storefront path and three further slugInfo calls as other identities each raised HitCount by 1 without adding records. Permalinks that resolve (category, product, content page, home) changed nothing. Source agrees: CompositeSeoResolver.FindSeoAsync publishes SeoInfoNotFoundEvent when no resolver returns a record, SeoInfoNotFoundEventHandler enqueues SaveBrokenLinkJob when Seo.BrokenLinkDetection.Enabled (default true). Consequence for any test or probe: calling slugInfo or POST /api/seoinfos/search with a permalink that does not resolve is a WRITE.
