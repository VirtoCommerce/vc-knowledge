---
id: KB-723ECFF9
subject: In the Admin Shell MCP screens the relative dates of Connected agents stay English in ar, he and ja, and after an in-session language switch the blade toolbar Refresh label can keep the previous language
plane: experiential
question: Are the Connected agents dates and the blade toolbar translated when the Admin Shell language changes?
questions:
  - text: Why does Connected agents show '10 minutes ago' in Arabic?
  - text: Admin Shell toolbar button shows Hebrew text in Japanese UI
status: active
appliesTo:
  - axis: module
    value: mcp
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: /apps/admin-shell#/mcp-agents
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; vc-shell=2.10.4
    defect: suspected
    at: 2026-11-10T15:38:49.964Z
    by: session:40688c0a
    who: Lenajava1
---
vcst_qa 15:25-15:35Z as the super-admin in {BACK_URL}/apps/admin-shell (vc-shell 2.10.4), language changed via Account > Language (15 languages offered). On #/mcp-agents the Last used / Connected date cells read "10 minutes ago", "about 1 hour ago", "1 day ago" in English while the rest of the screen was Arabic, Hebrew or Japanese. After switching Hebrew -> Japanese without a reload, the Connected agents blade toolbar button kept the Hebrew label "רענון" (Refresh) while menu, headers and buttons were Japanese. In ar and he the layout mirrors (section rail on the right, Disconnect and switches on the left, inside the viewport). No raw translation keys were seen on #/mcp-approvals, #/mcp-audit, #/mcp-tools, #/mcp-agents in ar, he, ja or de. Grids sat on their loading skeleton 10-20 s after a language switch although the api/mcp search requests had answered 200.
