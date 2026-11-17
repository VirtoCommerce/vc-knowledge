---
id: KB-C6E93B3C
subject: After a database page group is published a second time, GET grouped/{groupId}/content returns 200 with an empty body for both draft=true and draft=false
plane: experiential
question: What does GET /api/page-builder-pages/grouped/{groupId}/content return after a database page is edited and published again?
questions:
  - text: Why does a re-published Page Builder database page have no content?
  - text: Does publishing a Page Builder page a second time lose its content?
status: active
appliesTo:
  - axis: module
    value: pagebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/page-builder-pages/grouped/{id}/content
  - coordinate: POST /api/page-builder-pages/grouped/publishing/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    build: 3d715345b37f50ec
    conditions: platform=3.1039.19; module:VirtoCommerce.PageBuilderModule=3.1012.4
    defect: suspected
    at: 2026-11-17T12:00:53.244Z
    by: session:a434dd08
    who: kutasinaelena
---
As admin over REST: POST grouped (create), POST grouped/{id}/content (204), GET content draft=true returns the document, draft=false 404; POST grouped/publishing/{id}?publish=true (200) -> both draft=true and draft=false return the document. POST content again (204, a v2 draft): draft=true returns v2 and draft=false returns v1. POST publishing again (200) -> the group shows Published with one Published and one Archived version, but GET content returns 200 with an EMPTY body for both draft=true and draft=false. A third publish answers 400 "Draft page not found.". Reproduced twice on two groups. Not compared with a build before the hotfix, so not attributed to it; storefront rendering of the page was not checked.
