---
id: KB-CB838ECC
subject: Admin Shell Connected agents clips the MCP endpoint cell with no ellipsis or tooltip, and Agent audit log truncates cells with no tooltip
plane: experiential
question: Is the MCP endpoint column on the Admin Shell Connected agents screen (#/mcp-agents) readable, and do truncated cells on the Agent audit log (#/mcp-audit) expose their full value?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/mcp-agents
  - coordinate: /apps/admin-shell#/mcp-audit
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; viewport=1440x900 and 375x812; role=administrator
    defect: suspected
    at: 2026-11-10T15:48:17.523Z
    by: session:40688c0a
    who: Lenajava1
    note: "distinct: KB-89DF152E is about the menu group heading and a deep-link warning; this entry is about column clipping and missing tooltips inside the Connected agents and Agent audit log grids."
---
Admin Shell (vc-shell) as an administrator, desktop 1440 px. On Connected agents (#/mcp-agents) the "MCP endpoint" value is a code element 158 px wide in a 117 px slot styled overflow:hidden; text-overflow:clip, with no title attribute — the visible text ends mid-host, so the part that tells /admin/mcp from /ucp/mcp is cut and cannot be recovered by hovering. On Agent audit log (#/mcp-audit) 138 cells per page truncate with an ellipsis but carry no title, and the Date column shows only the date (time cut) at 1440 px. At 375 px the floating Refresh button covers the first switch on Agent tools at first paint until scrolled. Layout only; accessibility was out of scope for this run.
