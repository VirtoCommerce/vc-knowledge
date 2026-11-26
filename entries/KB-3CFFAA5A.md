---
id: KB-3CFFAA5A
subject: Site Builder MCP get_publish_readiness with theme:true does not report that a theme publication removes a live custom section type, while the publish_theme dry run warns of it
plane: experiential
question: Does get_publish_readiness theme:true warn that a theme publication will remove a live custom section type whose file is gone, as the publish_theme dry run does?
questions:
  - text: Which Site Builder MCP tool warns when publishing the theme drops a live custom section type?
  - text: Is get_publish_readiness for the theme the same as the publish_theme dry run?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    build: 1b6e861ce41cd6cb
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-6088; store=B2B-store
    defect: suspected
    at: 2026-11-26T13:59:51.308Z
    by: session:c8a6829d
    who: ivan-kalachikov
    note: "distinct: KB-E388859B is about which pages publish_theme's dry run lists (pre-fix); this is about get_publish_readiness missing the removed-live-type warning and the empty new_settings/removed_settings fields — a different tool and a different field."
---
B2B-store with a live custom section type whose live file had been deleted through the Content API and no theme drafts: get_publish_readiness {theme:true} answered only nothing-to-publish ("The theme has no drafts: a publication would put out the live files again."), while publish_theme dryRun on the same state showed a change row <theme>/custom-sections/<t>.json published → removed and the warning "Section type '<name>' (<t>) will be removed … this publication leaves it out", naming the pages that place it. Source at the build: readiness reads the theme through PublicationDryRuns.CheckThemeAsync only; the removed-type warning (DescribeRemovedSectionTypesAsync) lives in ThemePublishingTools. In the same build the publish_theme dry run's placed_sections.new_settings and removed_settings stay empty for a changed custom type (only change_section_type fills them); the setting changes appear in changes[].
