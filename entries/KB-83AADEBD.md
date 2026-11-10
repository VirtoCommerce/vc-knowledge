---
id: KB-83AADEBD
subject: /admin/mcp refuses a foreign, null, duplicated or other-host Origin with 403 before authentication, accepts a body up to exactly 1 MiB (413 above, empty body) and answers malformed JSON-RPC with 400 -32600, never 500
plane: experiential
question: How does /admin/mcp handle a cross-origin request, an oversized body and a malformed JSON-RPC message?
questions:
  - text: Does the MCP endpoint check the Origin header?
  - text: What is the request body size limit of the MCP endpoint?
  - text: What does /admin/mcp return for invalid JSON or a JSON-RPC batch?
  - text: Which JSON-RPC error does tools/call without a name return?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.mcp 3.1000.0-pr-1-df74
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /admin/mcp
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    at: 2026-11-10T14:20:56.484Z
    by: session:40688c0a
    who: Lenajava1
---
2026-11-10 with a valid operator token: Origin absent or equal to {BACK_URL} -> 200; Origin of another host, of the storefront host, http:// scheme of the own host, 'null', two Origin headers or two origins in one header -> 403 {"error":"forbidden","error_description":"Requests from this origin are not allowed to this MCP server."}, also without a token (checked before auth). Body of exactly 1 048 576 bytes -> 200; 1 048 577 bytes or 2 MiB (also chunked) -> 413 with an empty body. Truncated JSON, a non-JSON-RPC object, jsonrpc 1.0, missing method, numeric method, empty body, JSON null, a batch array -> 400 application/json {"error":{"code":-32600,...},"id":null} (messages quote the .NET parser, e.g. 'could not be converted to ModelContextProtocol.Protocol.JsonRpcMessage'); text/plain -> 415; unknown method -> -32601; tools/call with params as a string or without a name -> -32603 'An error occurred.' (not -32602).
