---
id: KB-6AF323AC
subject: The Agent approvals "What will change" table for a Site Builder publish_page shows only status Draft -> Published, not the content that goes live
plane: experiential
question: What does Agent approvals show as the diff of an agent's Site Builder publish_page request?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/mcp-approvals
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    build: 59e2041e3d7880ca
    conditions: module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-1129; module:VirtoCommerce.MCP=3.1001.0
    defect: VCST-7019
    at: 2026-11-25T18:30:08.143Z
    by: session:26322a1f
---
A publish_page request for a landing page whose draft held three new sections and an edited subtitle opened in Admin Shell #/mcp-approvals with a "What will change" table of one row: field status, Now Draft, After Published. No section, text or setting change of the draft is listed, before or after approval ("What changed" shows the same single row).
