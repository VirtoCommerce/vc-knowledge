---
id: KB-9F24D8EE
subject: Site Builder MCP page doors do not warn about a custom- section type with an invalid name (capital letter, trailing hyphen), while a valid-named missing type is warned on every door
plane: experiential
question: Do create_page, edit_page_draft, publish_page dry run and get_publish_readiness warn about a custom- section type whose name is invalid?
questions:
  - text: Is a custom-Agent-Test-x section type treated as a missing type by Site Builder MCP?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest
  - axis: tool
    value: create_page
anchors:
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; module:VirtoCommerce.MCP=3.1001.0; store=B2B-store
    defect: VCST-7046
    at: 2026-11-26T17:35:01.840Z
    by: session:c8a6829d
    who: ivan-kalachikov
    note: "distinct: About invalid custom- type names going unwarned on the page doors; the held entries are about readiness language prefixes, removed live types and publish_theme placed_sections."
---
create_page (dry and real) with sections custom-Agent-Test-<6>, custom-agent-test-<6>- and a valid never-created control custom-agent-test-missing-<6>: every warning names only the control ("places custom section types the theme does not hold"); the two invalid names appear in no warning. publish_page dryRun and get_publish_readiness {page_id} likewise name only the control; edit_page_draft adding the capital-letter type again returns warnings []. No call is refused. Expected per the custom-sections contract: a custom-prefixed type the theme cannot hold is a missing type and is warned.
