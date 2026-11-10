---
id: KB-A9F4CF19
subject: On /admin/mcp, answering the reset_platform_cache input_required question with action accept runs the reset once and audits one Succeeded row; the question round is not audited
plane: experiential
question: What happens when a 2026-07-28 MCP client accepts the confirmation of reset_platform_cache on /admin/mcp?
questions:
  - text: Does accepting the MCP elicitation confirmation actually reset the platform cache?
  - text: How is an accepted confirmation audited in the MCP audit log?
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
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; module:VirtoCommerce.SystemOperations=3.1003.0-pr-5-37af
    at: 2026-11-10T14:31:50.700Z
    by: session:40688c0a
    who: Lenajava1
    note: "distinct: KB-84893FFF records only the ask and the Decline path; this entry records the Accept path (the tool runs, result shape, Succeeded audit), which nobody had observed."
---
2026-11-10 14:25Z, operator (IT Platform Operator role) authorization-code token for client claude-admin-qa. tools/call reset_platform_cache {reason} with headers MCP-Protocol-Version 2026-07-28, Mcp-Method, Mcp-Name and _meta clientCapabilities {elicitation:{form:{}}} -> resultType input_required (message "Reset the platform cache now? ... Reason: <reason>"). The same call repeated with inputResponses.confirmation {action:"accept",content:{confirm:true}} and the returned requestState -> resultType complete, text {"reset":true,"at":...,"instance":"<one pod>","reason":...} (no _meta.trace_id on this success result). Audit: one new row outcome Succeeded, 9 ms, client claude-admin-qa, arguments with the reason (5 -> 6 rows; the input_required round adds none). /health stayed 200. The result names only the instance that ran the call although 2 instances run. Executed exactly once (shared stand).
