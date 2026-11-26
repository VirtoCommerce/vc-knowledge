---
id: KB-F9937AFE
subject: "A NEW Site Builder theme draft (no published copy) cannot be thrown away through Site Builder routes: discard-draft answers 409 and designer/discard answers 200 but leaves the draft"
plane: experiential
question: How is a Site Builder theme draft removed when the theme file has never been published?
questions:
  - text: "Why does POST /api/site-builder/discard-draft answer 409 'Nothing is published: the draft is the only copy.'?"
  - text: Does POST /api/site-builder/designer/discard remove a theme file draft?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/discard-draft
  - coordinate: POST /api/site-builder/designer/discard
  - coordinate: DELETE /api/content/themes/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    defect: suspected
    at: 2026-11-19T20:11:55.144Z
    by: session:07a3812a
    who: Lenajava1
    note: "distinct: KB-6619E4FA covers a draft over a PUBLISHED theme file (discard-draft 204); this is a draft with no published copy, where discard-draft is refused 409 and designer/discard silently keeps it."
  - method: observation
    deployment: vcst_dev
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-1129; module:VirtoCommerce.Content=3.1008.0; store=Electronics; role=admin
    at: 2026-11-25T23:49:26.784Z
    by: session:c8a6829d
    bodyHash: 3ddb3bdf6bf9
    note: "Held for a never-published BUILT-IN file: Electronics sections/header-group.json draft written by POST /api/site-builder/save?draft=true; POST designer/discard with themeFiles [\"sections/header-group.json\"] and [\"default/sections/header-group.json\"] both answered 200 {saveId}, theme/status kept it in drafts+newDrafts; DELETE /api/content/themes/Electronics?urls=/default/sections/header-group.json-draft (204) removed it. Exception on this build: a never-published CUSTOM SECTION TYPE draft (custom-sections/<t>.json) IS removed by designer/discard with the default/ path (ThemeDraftService.GetDiscardStatus treats custom-section types as discardable)."
---
B2B-store at a moment its theme had no sections/header-group.json published: a header-group.json draft written by POST /api/site-builder/save?draft=true was refused by POST /api/site-builder/discard-draft?storeId=B2B-store&path=default/sections/header-group.json&type=themes&theme=default with 409 "Nothing is published: the draft is the only copy." POST /api/site-builder/designer/discard?storeId=B2B-store with {themeFiles:["default/sections/header-group.json"]} and with {themeFiles:["sections/header-group.json"]} each answered 200 {saveId} but the draft still read back from designer/theme-file and template?draft=true. The draft blob was removed only with DELETE /api/content/themes/B2B-store?urls=/default/sections/header-group.json-draft (204).
