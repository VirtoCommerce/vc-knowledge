---
id: KB-AEDC697F
subject: "POST /api/site-builder/save with draft=true writes a theme section draft without the schema-catalog check designer/save enforces: no catalogHash needed and a colour token the storefront lacks is stored"
plane: experiential
question: Does POST /api/site-builder/save draft=true validate theme files like POST designer/save does?
questions:
  - text: Can a theme draft be saved without catalogHash through the legacy save route?
  - text: Which Site Builder save route skips theme validation?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/save
  - coordinate: POST /api/site-builder/designer/save
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store
    defect: suspected
    at: 2026-11-19T14:21:55.614Z
    by: session:51ac2fe3
    who: Lenajava1
    note: "distinct: KB-620E2625 says designer/save and theme/publish need a catalogHash; this is the legacy /api/site-builder/save draft=true route, which needs none and skips the schema-catalog check entirely."
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    at: 2026-11-19T14:56:09.754Z
    by: session:51ac2fe3
    bodyHash: 8a796fb8629b
    who: Lenajava1
    note: "Re-reproduced 1/1 at 14:54-14:56Z, publication 16: legacy save no-hash 200, draft listed; designer/save 400 with and without hash; the SAME legacy call WITH catalogHash=<current hash> is refused 400 with the value error, so the check exists and is skipped only when catalogHash is omitted. Source at 28d00e0: DesignerContentService.ValidateThemeFilesInternalAsync returns before themePublisher.ValidateFilesAsync when catalogHash is empty, and SaveTemplates never requires one (designer/save does). A unit test (SaveTemplates_NoCatalogHash_DoesNotCheckTheFiles) codifies the skip, and the PR lists catalogHash as optional on /save. String content again stored double-encoded. Restored with discard-draft (204), drafts [] and publication 16 unchanged."
---
As admin on B2B-store: POST /api/site-builder/save?storeId=B2B-store&theme=default&draft=true (no catalogHash) with files = JSON string of [{path:"default/sections/header-group.json", type:"themes", content:<object with announcement-bar background "color_agent_test_6571_bogus">}] -> 200 empty body; designer/theme-file then returned that content and theme/status listed the header draft. The same file sent through POST designer/save is refused 400 without catalogHash ("Theme files are checked against the storefront's schema catalog") and 400 with it (value the storefront does not have). With content sent as a JSON string instead of an object, the draft was stored double-encoded (content is a JSON string literal). The invalid draft is caught later: POST theme/publish -> 400 naming the file, so it does not reach the storefront, but it blocks every later Publish theme until replaced.
