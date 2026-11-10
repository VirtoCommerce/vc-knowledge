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
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; vc-shell=2.10.4; browser=firefox156; viewport=1440x900 and 375x812 and 1920x1080; role=administrator
    at: 2026-11-10T16:12:52.330Z
    by: session:40688c0a
    bodyHash: dc565e643ad2
    defect: suspected
    who: Lenajava1
    note: "Re-measured 3/3 on Firefox. Refinement: the endpoint cell is a 2-line clamp (119x40 px) and the URL breaks at hyphens into three lines, so the third line (admin/mcp or ucp/mcp) is clamped away entirely; Firefox paints a line-clamp ellipsis at the end of line 2, Chromium a hard cut. Data call POST /api/mcp/connected-agents/search returns the full resources value. At 1920x1080 both the endpoint (199 px slot) and the audit Date (195 px) show in full, so the cut is width-driven. Audit page 1-50 of 919: 162 of 350 cells truncated, none titled; Date shows '11/10/2026, 5:' only. On a 375 px phone the endpoint column is not rendered at all. F12: the fixed Refresh pill covers 660 of 720 px2 of the 'Check pending migrations' switch at first paint; the end of the list scrolls clear of it."
---
Admin Shell (vc-shell) as an administrator, desktop 1440 px. On Connected agents (#/mcp-agents) the "MCP endpoint" value is a code element 158 px wide in a 117 px slot styled overflow:hidden; text-overflow:clip, with no title attribute — the visible text ends mid-host, so the part that tells /admin/mcp from /ucp/mcp is cut and cannot be recovered by hovering. On Agent audit log (#/mcp-audit) 138 cells per page truncate with an ellipsis but carry no title, and the Date column shows only the date (time cut) at 1440 px. At 375 px the floating Refresh button covers the first switch on Agent tools at first paint until scrolled. Layout only; accessibility was out of scope for this run.
