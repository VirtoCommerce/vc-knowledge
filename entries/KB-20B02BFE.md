---
id: KB-20B02BFE
subject: "On MCP protocol 2026-07-28, /admin/mcp is stateless: initialize is refused, and every request needs the Mcp-Method header (Mcp-Name for tools/call and resources/read) and _meta io.modelcontextprotocol/protocolVersion"
plane: experiential
question: How must an MCP client call /admin/mcp with protocol version 2026-07-28?
questions:
  - text: Why does /admin/mcp answer Missing required Mcp-Method header?
  - text: Does /admin/mcp accept initialize on protocol 2026-07-28?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /admin/mcp
  - coordinate: GET /connect/authorize
evidence:
  - method: observation
    deployment: vcst_dev
    build: 59e2041e3d7880ca
    conditions: module:VirtoCommerce.MCP=3.1001.0; platform=3.1100.0
    at: 2026-11-25T18:30:27.846Z
    by: session:26322a1f
---
With an authorization-code + PKCE token (public OAuth client, resource = {BACK_URL}/admin/mcp, scope openid offline_access; adding profile was refused as a scope the client may not use) and header mcp-protocol-version: 2026-07-28: a request without params._meta["io.modelcontextprotocol/protocolVersion"] got -32602 "Requests using protocol version '2026-07-28' must include '_meta/io.modelcontextprotocol/protocolVersion'"; without an Mcp-Method header -32020 "Missing required Mcp-Method header."; resources/read without Mcp-Name (= the uri) -32020 "Missing required Mcp-Name header."; and initialize -32601 "Method 'initialize' is not available on protocol version '2026-07-28'". tools/list, tools/call and resources/read sent directly with those headers and _meta succeed. The authorize request redirected straight to the client's callback with a code after the back-office sign-in, with no consent page.
