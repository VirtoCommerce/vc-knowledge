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
---
As admin on store Electronics (theme folder default, site history enabled), with the catalog hash from default/catalogs/current.json and the eTag from GET designer/theme-file: POST /api/site-builder/designer/save with themeFiles [{path:"default/agent-test-<r>.json", ...}] answered 200 with warnings [], 5 of 5 paths (also a <script>/onclick body, default/agent-test-<r>/bar.json and default/sections/agent-test-<r>.json). The Content API listing showed /default/<path>-draft (published false) and GET /api/site-builder/history?item=theme:default/<path> showed the Save as a version. GET /api/site-builder/theme/status listed none of them in drafts or newDrafts (it lists only the fixed theme parts, templates/<key>.json and custom-sections/<type>.json). Removing them: POST /api/site-builder/discard-draft?type=themes answered 409 "Nothing is published: the draft is the only copy." and POST /api/site-builder/designer/discard answered 200 {saveId} but kept the draft (5/5); only DELETE /api/content/themes/Electronics?urls=/default/<path>-draft (204) removed it. Controls in the same run: default/config/settings_data.json is listed in theme/status drafts and removed by designer/discard; a new default/custom-sections/custom-<t>.json is listed in drafts and newDrafts and removed by designer/discard or discard-draft (204). Paths without the theme folder, or under snapshots/, catalogs/ or a non-builder config/ file, are refused 400. Both designer/save and designer/discard run the same path validator (a deny-list); the discard keeps the stray file because a never-published draft that is not a custom section type is its only copy.
