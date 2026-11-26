---
id: KB-62E6AE6D
subject: Site Builder MCP get_publish_readiness for a multilanguage page repeats the missing/draft-only custom-type findings per language without naming the language
plane: experiential
question: Does site-builder MCP get_publish_readiness name the language in its missing and draft-only custom section type findings for a multilanguage page?
questions:
  - text: get_publish_readiness multilanguage_page_id findings language prefix
  - text: why are readiness warnings duplicated for each language version of a site builder page
status: active
appliesTo:
  - axis: module
    value: virtocommerce.sitebuilder
  - axis: surface
    value: rest
  - axis: tool
    value: get_publish_readiness
anchors:
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; platform=3.1100.0; languages=en-US,de-DE
    defect: suspected
    at: 2026-11-26T15:58:14.650Z
    by: session:c8a6829d
    who: ivan-kalachikov
    note: "distinct: About the multilanguage_page_id target and the missing language prefix on per-language custom-type findings. KB-3CFFAA5A is about theme:true and a removed live type, and KB-E388859B is about publish_theme placed_sections."
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; module:VirtoCommerce.MCP=3.1001.0; languages=en-US,de-DE
    at: 2026-11-26T17:29:59.899Z
    by: session:c8a6829d
    bodyHash: 38aa42e77194
    defect: VCST-7046
    who: ivan-kalachikov
---
On SiteBuilder 3.1003.0-pr-5-01b3, get_publish_readiness with multilanguage_page_id for a two-language page (en-US, de-DE) placing one missing and one draft-only custom section type returns one section-type-missing and one section-type-unpublished finding per language version. The titles are identical; only object_id tells them apart. On the same build the publish_page dry run with all_languages and create_page prefix each warning with its language ("en-US: …", "de-DE: …"). In source, the multilanguage readiness branch (CheckLanguageVersionAsync) adds no prefix. Two separate lanes saw it.
