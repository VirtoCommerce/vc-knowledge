---
id: KB-E58CB6BE
subject: Site Builder MCP edit_template for a template key the theme does not hold, without copy_of or version_token, announces create_template from a seed with no error and no warning naming copy_of
plane: experiential
question: What does edit_template answer for a new template_key passed without copy_of or version_token?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest
  - axis: tool
    value: edit_template
anchors:
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; module:VirtoCommerce.MCP=3.1001.0; store=B2B-store
    defect: VCST-7046
    at: 2026-11-26T17:35:01.983Z
    by: session:c8a6829d
    who: ivan-kalachikov
    note: "distinct: About edit_template creating a template from a seed without asking for copy_of; KB-20010B2D is about create_page multilanguage_page_copy's object_name."
---
edit_template {store_id, template_key: product.agent-test-<6>, operations: [], dryRun: true} answered not isError, action create_template, a seeded product-info section, and one warning only ("gets pages only through a template rule: add one with edit_template_rules"); neither copy_of nor version_token is named. theme/status listed no draft for the key after the dry run. The tool description says a new key creates the template and to pass copy_of with it, so the seed-without-asking is the open defect.
