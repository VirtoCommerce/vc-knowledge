---
id: KB-E3F1CF02
subject: Connected agents "Last used" is the last token issue or refresh, not the last MCP call, and "Active sign-ins" counts authorization-code grants, not refreshes
plane: experiential
question: What do the Last used and Active sign-ins columns of Admin Shell Connected agents measure?
questions:
  - text: Does Connected agents Last used move when the agent calls a tool?
  - text: Why does it-operator-qa show 256 active sign-ins?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: /apps/admin-shell#/mcp-agents
  - coordinate: POST /api/mcp/connected-agents/search
  - coordinate: DELETE /api/mcp/connected-agents/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    at: 2026-11-10T07:38:15.406Z
    by: session:d6dc0a6d
    who: Lenajava1
    note: "distinct: KB-EB2CD3ED covers what Disconnect revokes; this entry is what the Last used and Active sign-ins columns measure."
---
POST /api/mcp/connected-agents/search (and #/mcp-agents) for an /admin/mcp agent: lastUsedDate stayed at 18:59:58Z after a tools/call at 19:12:22Z, then moved to 19:12:30Z on a refresh_token grant. For another agent, calls from 19:09:31 to 19:11:50 left it at 19:09:27Z, the time of its refresh. activeSessionCount was 2 after two authorization-code grants for the same user and client, and stayed 2 after a refresh. Disconnect (DELETE /api/mcp/connected-agents/{id}, 204) removes the row, all of that user's sign-ins answer 401, and a new authorize shows the consent page again.
