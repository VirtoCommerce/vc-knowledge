---
id: KB-023FBDCC
subject: On /admin/mcp a tools/call whose unknown tool name is 129 characters or longer gets the normal -32602 "Unknown tool" error but leaves no audit row; names up to 128 characters are audited as Error
plane: experiential
question: Is a tools/call with an over-long unknown tool name audited on /admin/mcp?
questions:
  - text: Does the MCP audit log record calls with tool names longer than 128 characters?
  - text: What happens to the audit row when an agent calls an unknown tool with a very long name?
status: active
appliesTo:
  - axis: module
    value: mcp
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /admin/mcp
  - coordinate: POST /api/mcp/audit/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    defect: suspected
    at: 2026-11-10T15:12:44.913Z
    by: session:40688c0a
    who: Lenajava1
    note: "distinct: KB-77B3269C is about switched-off tools and the kill switch setting; this is about audit rows missing for unknown tool names over 128 characters."
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; role=IT Platform Operator; client=claude-admin-qa
    at: 2026-11-10T15:58:04.593Z
    by: session:40688c0a
    bodyHash: 524367ffe231
    defect: suspected
    who: Lenajava1
    note: "Re-confirmed 3 rounds at 15:50Z plus 2 single calls at 15:52Z: 127/128-char unknown names audited as Error; 129, 130, 250, 300, 614 not. App Insights shows the INSERT INTO McpAuditEntry dependency failing with SQL error 2628 (string would be truncated) on the same operation id while the request is 200; source: McpAuditEntryEntity.ToolName is nvarchar(128) and McpAuditWriter.WriteAsync catches and only logs the exception."
---
With an authorization-code+PKCE agent token (client claude-admin-qa, IT Platform Operator role) on vcst_qa at 15:12Z: tools/call with unknown tool names of 120, 128, 129, 130 and 250 characters all returned HTTP 200 with JSON-RPC error -32602 "Unknown tool: '<name>'" (same answer regardless of length, no 5xx). POST api/mcp/audit/search for that user from the start time returned rows only for the 120- and 128-character calls (outcome Error, full name stored) and the control list_modules (Succeeded); the 129, 130 and 250 calls left no row, even after 15 s. Consistent with a 128-character column limit making the audit insert fail silently.
