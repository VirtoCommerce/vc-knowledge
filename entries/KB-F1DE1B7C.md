---
id: KB-F1DE1B7C
subject: Page Builder create endpoints return HTTP 500 with the raw SQL truncation message (database and table names) when Name exceeds 1024 characters
plane: experiential
question: What does creating a Page Builder page or Multilanguage Page with a Name over 1024 characters return?
questions:
  - text: Is there a length validation on Page Builder page names?
  - text: Why does page create return 500 for a long name?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-api
anchors:
  - coordinate: POST /api/page-builder-multilanguage-pages
  - coordinate: POST /api/page-builder-pages/create-group-page
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; platform=3.1085.0
    defect: suspected
    at: 2026-10-23T13:00:01.200Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    conditions: module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-175-3d2f; platform=3.1085.0
    at: 2026-10-27T19:04:58.695Z
    by: session:b715861d
    bodyHash: ab23fdcd430d
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: module:VirtoCommerce.PageBuilderModule 3.1039.0-pr-175-3d2f vs 3.1036.0-pr-175-a3bc; virtocommerce.pagebuildermodule 3.1039.0-pr-175-3d2f vs 3.1036.0-pr-175-a3bc; virtocommerce.pages 3.1011.0-pr-23-e50f vs 3.1011.0-pr-23-4bcd
    note: "On PageBuilderModule 3.1039.0-pr-175-3d2f a 1025-character Name returns 400 \"Name must be at most 1024 characters.\" (no SQL text) on POST and PUT /api/page-builder-multilanguage-pages, POST /api/page-builder-pages/create-group-page, POST /api/page-builder-pages/grouped and PUT /api/page-builder-pages/grouped; 1024 characters -> 200. The ML root permalink is now validated too: 2049 chars, whitespace, tab, zero-width space, each of ? # & % \\ \" < > { } | ^ `, '//', '/./' and '/../' -> 400 with the permalink rule text; 2048 chars and non-Latin letters -> 200."
---
POST /api/page-builder-multilanguage-pages with a 1024-character Name -> 200; with 1025 characters -> 500 {"message":"String or binary data would be truncated in table '<db>.dbo.PageBuilderMultilanguagePage', column 'Name'. Truncated value: ..."}. POST /api/page-builder-pages/create-group-page with an 1100-character name -> the same 500 for table GroupedPageBuilderPage, so the gap pre-dates the multilanguage feature. A 1124-character permalink is accepted (200). Empty or whitespace Name/Permalink -> 400 "StoreId, Name and Permalink are required." and the admin blade blocks them client-side.
