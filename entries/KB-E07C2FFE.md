---
id: KB-E07C2FFE
subject: On /admin/mcp, publish_page (no dryRun) creates a pending change in Agent approvals; approving it needs mcp:approve plus builder:read on the page's store and builder:publish, not mcp:approve alone
plane: experiential
question: What does publish_page on /admin/mcp do, and which permissions does the user who approves the publication in Agent approvals need?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: admin-mcp
anchors:
  - coordinate: /admin/mcp
evidence:
  - method: observation
    deployment: vcst_dev
    conditions: module:VirtoCommerce.SiteBuilder=3.1002.0-pr-2-c357; module:VirtoCommerce.MCP=3.1001.0; platform=3.1100.0; setting:MCP.Approval.AllowSelfApproval=false; protocol=2026-07-28
    at: 2026-11-25T16:33:17.135Z
    by: session:c8a6829d
---
With an authorization-code + PKCE token for a user whose role has builder:* on one store, tools/call publish_page with the expected_versions from its dry run returned a pending change instead of publishing; the page stayed unpublished on the storefront. The same user approving it was refused (self-approval off). A second user with only mcp:approve could not approve it; after adding builder:read on that store and builder:publish to the approver's role, the approval succeeded and the page went live on the storefront. A user with no builder permissions got 0 Site Builder tools from tools/list; the builder:* user got 27.
