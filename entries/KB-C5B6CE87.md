---
id: KB-C5B6CE87
subject: "Site Builder page lifecycle on a store without GitHub: history/status answers setupRequired (no error), multilanguage publish-all reports per-language skips with reason NoDraft, and a multilanguage page can be deleted only after its language versions are gone"
plane: experiential
question: How do Site Builder history, publish-all and multilanguage delete behave on a store with no GitHub repository?
questions:
  - text: What does GET /api/site-builder/history return on a store without git content?
  - text: What does publish-all return when no language has a draft?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/site-builder/history/status
  - coordinate: POST /api/site-builder-multilanguage-pages/{id}/publish-all
  - coordinate: DELETE /api/site-builder-multilanguage-pages/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; github-app=none
    at: 2026-11-19T13:56:04.700Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    at: 2026-11-19T20:12:59.032Z
    by: session:07a3812a
    bodyHash: 9e315bcd0925
    who: Lenajava1
    note: "Re-seen only for the last clause, from the shell page details blade (suite 123b SB-090/SB-093): Unpublish has no confirmation, POST /grouped/publishing/{id}?publish=false -> 200, the status chip returns to Draft and no unsaved-changes banner appears."
---
B2B-store (GitContentEnabled unset, no GitHub App): GET /api/site-builder/history/status -> {"status":"setupRequired"}; GET history?item=page:{groupId} -> {"status":"setupRequired","versions":[]}; GET history with no item -> 400 "Name at least one item: page:{groupId} or theme:{path}."; Admin Shell Sites > History shows "Not connected" with an explanation. Designer Save sends exactly one POST designer/save; Publish one POST grouped/publishing (no theme publish). Publish all in the UI toasts "Published: en-US, de-DE. Skipped (no draft): —."; calling POST multilanguage-pages/{id}/publish-all again -> {processedCultureNames:[], skipped:[{cultureName,reason:"NoDraft"}...]}. DELETE multilanguage-pages/{id} with versions -> 400 "has language versions: archive them instead."; after deleting the versions -> 204. Unpublish in the designer has no confirmation and the storefront answers 404 at once.
