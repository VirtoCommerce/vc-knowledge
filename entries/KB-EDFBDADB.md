---
id: KB-EDFBDADB
subject: "POST /api/site-builder/designer/save accepts any JSON path inside the store's theme folder that is not a builder file (default/<name>.json, default/<dir>/bar.json, default/sections/<name>.json): 200 unchecked, stored as a draft and recorded in history, but absent from theme/status and not removable by any Site Builder route"
plane: experiential
question: Does POST /api/site-builder/designer/save refuse a theme file path inside the store's theme folder that is not one of the files the builder edits, such as default/foo/bar.json?
questions:
  - text: Can designer/save create an arbitrary file in the theme folder?
  - text: Why does a theme draft not show in GET /api/site-builder/theme/status?
  - text: How do I remove a stray -draft file in a Site Builder theme folder?
  - text: Which theme file paths does designer/save allow?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/designer/save
  - coordinate: POST /api/site-builder/designer/discard
  - coordinate: POST /api/site-builder/discard-draft
  - coordinate: GET /api/site-builder/theme/status
  - coordinate: DELETE /api/content/themes/{id}
evidence:
  - method: observation
    deployment: vcst_dev
    build: 1b6e861ce41cd6cb
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-6088; module:VirtoCommerce.Content=3.1008.0; store=Electronics; role=administrator
    defect: suspected
    at: 2026-11-26T14:07:09.005Z
    by: session:c8a6829d
    who: ivan-kalachikov
    note: "distinct: KB-32BF9D08 is about a path WITHOUT the theme folder (now refused 400 on pr-5-6088) and KB-F9FA689C about discard of un-prefixed paths; this is a path WITH the store's theme folder that names no builder file, still saved 200 and then invisible to theme/status and un-discardable."
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; module:VirtoCommerce.Content=3.1008.0; store=Electronics; role=administrator
    at: 2026-11-26T15:52:46.251Z
    by: session:c8a6829d
    bodyHash: d03fb1b37100
    who: ivan-kalachikov
    contradicts: true
    contested: module:VirtoCommerce.SiteBuilder 3.1003.0-pr-5-01b3 vs 3.1003.0-pr-5-6088
    note: "On module VirtoCommerce.SiteBuilder 3.1003.0-pr-5-01b3 (later build than the entry's pr-5-6088) every theme-file door refuses a non-builder path inside the theme folder (default/agent-test-<r>.json, default/agent-test-<r>/bar.json, default/sections/agent-test-<r>.json, default/templates/Product.json, default/content/templates/product.json): GET designer/theme-file, POST designer/save, designer/discard, api/site-builder/save draft=true, discard-draft and history/{v}/restore each answer 400 {\"error\":\"\\\"<path>\\\" is not a theme file the builder saves (templates, custom section types, header and footer groups, settings, rules, brand, palettes).\"}; MCP restore_version returns isError with the same text. Nothing written: no Content API listing, no history version for the item, theme/status unchanged. Looks fixed on this build; the entry's observation was on pr-5-6088."
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; store=Electronics; role=admin
    at: 2026-11-26T17:55:50.295Z
    by: session:c8a6829d
    bodyHash: d03fb1b37100
    who: ivan-kalachikov
    contradicts: true
    contested: module:VirtoCommerce.SiteBuilder 3.1003.0-pr-5-01b3 vs 3.1003.0-pr-5-6088; role admin vs administrator; virtocommerce.sitebuilder 3.1003.0-pr-5-01b3 vs 3.1003.0-pr-5-6088
    note: "On pr-5-01b3, 5 non-builder paths in the theme folder (default/agent-test-<r>.json, default/agent-test-<r>/bar.json, default/sections/agent-test-<r>.json, default/templates/Product.json, default/content/templates/product.json) x designer/theme-file, designer/save (no eTag and a well-formed stale eTag), designer/discard, legacy save draft=true with and without catalogHash, discard-draft and history restore: all 400 '\"<path>\" is not a theme file the builder saves (...)'; theme/status and history count unchanged, no draft blob. A malformed eTag answers the eTag-format 400 first. Same store with DefaultThemeName=sections refuses sections/agent-test-<r>.json on every door and accepts sections/templates/product.json."
---
As admin on store Electronics (theme folder default, site history enabled), with the catalog hash from default/catalogs/current.json and the eTag from GET designer/theme-file: POST /api/site-builder/designer/save with themeFiles [{path:"default/agent-test-<r>.json", ...}] answered 200 with warnings [], 5 of 5 paths (also a <script>/onclick body, default/agent-test-<r>/bar.json and default/sections/agent-test-<r>.json). The Content API listing showed /default/<path>-draft (published false) and GET /api/site-builder/history?item=theme:default/<path> showed the Save as a version. GET /api/site-builder/theme/status listed none of them in drafts or newDrafts (it lists only the fixed theme parts, templates/<key>.json and custom-sections/<type>.json). Removing them: POST /api/site-builder/discard-draft?type=themes answered 409 "Nothing is published: the draft is the only copy." and POST /api/site-builder/designer/discard answered 200 {saveId} but kept the draft (5/5); only DELETE /api/content/themes/Electronics?urls=/default/<path>-draft (204) removed it. Controls in the same run: default/config/settings_data.json is listed in theme/status drafts and removed by designer/discard; a new default/custom-sections/custom-<t>.json is listed in drafts and newDrafts and removed by designer/discard or discard-draft (204). Paths without the theme folder, or under snapshots/, catalogs/ or a non-builder config/ file, are refused 400. Both designer/save and designer/discard run the same path validator (a deny-list); the discard keeps the stray file because a never-published draft that is not a custom section type is its only copy.
