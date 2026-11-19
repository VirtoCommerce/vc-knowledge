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
---
As admin on B2B-store: POST /api/site-builder/save?storeId=B2B-store&theme=default&draft=true (no catalogHash) with files = JSON string of [{path:"default/sections/header-group.json", type:"themes", content:<object with announcement-bar background "color_agent_test_6571_bogus">}] -> 200 empty body; designer/theme-file then returned that content and theme/status listed the header draft. The same file sent through POST designer/save is refused 400 without catalogHash ("Theme files are checked against the storefront's schema catalog") and 400 with it (value the storefront does not have). With content sent as a JSON string instead of an object, the draft was stored double-encoded (content is a JSON string literal). The invalid draft is caught later: POST theme/publish -> 400 naming the file, so it does not reach the storefront, but it blocks every later Publish theme until replaced.
