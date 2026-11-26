---
id: KB-3DD26D0A
subject: Site Builder MCP edit_page_draft warns about a missing or draft-only custom section type only when the edit newly places that type on the page; adding another section of a type the page already places returns warnings []
plane: experiential
question: Does edit_page_draft on /admin/mcp warn about a missing or draft-only custom section type when the page already places that type?
questions:
  - text: When does edit_page_draft return custom section type warnings?
  - text: Why does edit_page_draft add_section of a missing custom type return no warning?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest
  - axis: tool
    value: edit_page_draft
anchors:
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; module:VirtoCommerce.MCP=3.1001.0; store=B2B-store
    at: 2026-11-26T17:35:01.709Z
    by: session:c8a6829d
    who: ivan-kalachikov
    note: "distinct: About edit_page_draft's warnings for re-placing a type the page already holds; the held entries are about get_publish_readiness language prefixes, publish_theme placed_sections and readiness for removed live types."
  - method: source
    deployment: VirtoCommerce.SiteBuilder@3.1003.0-pr-5-01b3
    at: 2026-11-26T18:13:02.204Z
    by: session:c8a6829d
    bodyHash: fc66e3877030
    who: ivan-kalachikov
    note: "Source agrees: PublicationDryRuns.GetAddedSectionTypeFindingsAsync takes placed types after the edit minus placed types before it, so a repeat placement yields no finding."
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; module:VirtoCommerce.MCP=3.1001.0; store=B2B-store
    at: 2026-11-26T18:29:51.251Z
    by: session:c8a6829d
    bodyHash: fc66e3877030
    who: ivan-kalachikov
    note: "SBMCP-018: edit_page_draft add_section of a newly placed missing type and of a newly placed draft-only type each warned, not refused; create_page, language_version, multilanguage_page_copy and publish_page dry runs warned for both types with a language prefix."
---
On B2B-store, a page (en-US version of a two-language page) already placing a missing type TM and a draft-only type TD: edit_page_draft add_section TM, then add_section TD, each answered warnings [] (not refused, saved). On the same page an add_section of a NEW missing type (never placed before) answered the usual warning "places custom section types the theme does not hold (...): it can still be published, but the storefront shows nothing in their place". create_page, language_version, multilanguage_page_copy and publish_page dry runs on the same page warn about TM and TD every time. So edit_page_draft's warning covers only types the edit introduces to the page.
