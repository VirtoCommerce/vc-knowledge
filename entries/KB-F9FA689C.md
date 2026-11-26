---
id: KB-F9FA689C
subject: POST /api/site-builder/designer/discard silently no-ops (200 {saveId}) when a themeFiles path lacks the theme folder — e.g. the in-theme form theme/status returns — for any theme file; only "<theme>/<path>" discards
plane: experiential
question: Does POST /api/site-builder/designer/discard accept the path form GET theme/status returns (custom-sections/<t>.json, config/settings_data.json), or only the theme-prefixed form?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/designer/discard
  - coordinate: GET /api/site-builder/theme/status
evidence:
  - method: observation
    deployment: vcst_dev
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-1129; module:VirtoCommerce.Content=3.1008.0; store=Electronics; role=admin; theme=default
    at: 2026-11-25T23:49:26.993Z
    by: session:c8a6829d
---
On Electronics as admin: GET /api/site-builder/theme/status lists drafts by their path in the theme (custom-sections/custom-agent-test-<x>.json, config/settings_data.json). Feeding that exact string to POST /api/site-builder/designer/discard?storeId=Electronics {themeFiles:["custom-sections/<t>.json"]} answered 200 {saveId} but the draft stayed: theme/status still listed it and designer/theme-file?path=default/custom-sections/<t>.json still returned its content (3 attempts, 2 files: a new custom section type created by MCP create_section_type, and a draft of the published config/settings_data.json). The same call with "default/custom-sections/<t>.json" / "default/config/settings_data.json" removed the draft (status empty, settings_data back to live content). A path with an arbitrary first segment ("no-such-theme/sections/nothing.json") also answers 200. Source at PR head: the first path segment is taken as the theme name (SiteBuilderContentPaths.GetThemeName), the path check only requires a '/' (SiteBuilderContentFileValidator IsInsideAThemeFolder), the draft lookup in theme "custom-sections" finds nothing (DraftDiscardStatus.NoDraft) and DesignerContentService.DiscardAsync returns {saveId} without per-file outcome. Follows: a client must prefix theme/status paths with the store's theme folder before discarding; the 200 is not proof anything went.
