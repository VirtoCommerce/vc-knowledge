---
id: KB-B70CDB22
subject: "UCP rejects an unknown store_id differently per transport: MCP search_products returns a UCP invalid_request isError result, REST /ucp/v1/catalog/search returns HTTP 200 with the XCatalog GraphQL envelope (NULL_REFERENCE)"
plane: experiential
question: What does UCP return for a store_id that does not exist, on the MCP tool versus the REST endpoint?
status: active
appliesTo:
  - axis: module
    value: ucp
  - axis: surface
    value: ucp-mcp
  - axis: surface
    value: ucp-rest
anchors:
  - coordinate: POST /ucp/mcp
  - coordinate: POST /ucp/v1/catalog/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:40:34.812Z
    by: session:624d35a1
    who: kutasinaelena
---
UCP 3.1010.0 (XCatalog 3.1023.0), anonymous, 2026-10-13. MCP tools/call search_products {store_id:"NO-SUCH-STORE-XYZ", query} -> HTTP 200 SSE, no JSON-RPC error, result.isError=true, structuredContent {is_error:true, code:"invalid_request", status_code:400, message:"store_id does not identify an existing store."}, _meta.source="ucp", _meta.error_code="invalid_request", plus a "Trace ID: <32 hex>" text part equal to _meta.trace_id. The same bogus store on REST POST /ucp/v1/catalog/search (with or without context) -> HTTP 200 with the raw GraphQL envelope errors[0].extensions.code="NULL_REFERENCE", data.products=null, and an X-Trace-Id header; no UCP error code. So MCP validates the store before calling xAPI while REST passes it through and the failure surfaces as an xAPI envelope (consistent with the 3.1005 release note that REST XAPI failures return HTTP 200 + original envelope). An unknown currency (ZZZ) on MCP gives isError with structuredContent = the XCatalog envelope (INVALID_OPERATION) and _meta.source="XCatalog". The docs Error model lists invalid_request but does not say which transport validates store_id; a test must not assume the two transports agree.
