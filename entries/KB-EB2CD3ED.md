---
id: KB-EB2CD3ED
subject: An /admin/mcp agent token is also accepted by platform REST and back-office /graphql as the user, and Connected agents Disconnect revokes it on all three plus its refresh token
plane: experiential
question: Is an OAuth access token issued for the /admin/mcp resource also accepted by platform REST and /graphql, and does Disconnect revoke it there?
questions:
  - text: Does the MCP agent token audience (RFC 8707) limit it to the MCP endpoint only?
  - text: After Disconnect in Admin Shell Connected agents, does the agent's token still work on REST or GraphQL?
  - text: Does a pending change survive when its requesting agent is disconnected?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: /admin/mcp
  - coordinate: GET /api/platform/security/currentuser
  - coordinate: POST /graphql
  - coordinate: /apps/admin-shell#/mcp-agents
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    at: 2026-11-09T18:51:12.997Z
    by: session:d6dc0a6d
    who: Lenajava1
---
With an authorization-code + PKCE token for client claude-admin-qa, aud = {BACK_URL}/admin/mcp: GET /api/platform/modules and GET /api/platform/security/currentuser returned 200 as the signed-in user, and POST /graphql { me } returned that user. The same token is refused on /ucp/mcp ("not issued for this MCP resource"), so audience binding holds between MCP scopes but not between MCP and the platform's own REST/GraphQL. For a non-admin user (IT Platform Operator role) Disconnect in Admin Shell #/mcp-agents made the same token return 401 on /admin/mcp ("The OAuth session has ended"), 401 on REST and 401 on back-office /graphql, and refresh gave 400 invalid_grant. A restart_platform pending change that agent created stayed Pending after Disconnect. Note GET /api/platform/security/currentuser answers 200 with an empty anonymous body when no token is sent, so only a 401 there proves rejection.
