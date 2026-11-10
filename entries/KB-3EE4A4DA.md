---
id: KB-3EE4A4DA
subject: "A /ucp/mcp buyer token cannot read or change another buyer's cart or order: carts are refused (Access denied / buyer_context_mismatch 403), orders answer order_not_found, and the refusals are audited Failed"
plane: experiential
question: Can a buyer's UCP MCP token read or modify another buyer's (other organization) cart or order on /ucp/mcp?
questions:
  - text: Does UCP MCP enforce buyer isolation between organizations?
  - text: What does get_cart return for a cart id that belongs to another buyer?
  - text: Does track_order reveal an order of another buyer by order number?
status: active
appliesTo:
  - axis: buyer
    value: authenticated-b2b
  - axis: module
    value: virtocommerce.ucp 3.1011.0-pr-13
  - axis: surface
    value: ucp
anchors:
  - coordinate: POST /ucp/mcp
  - coordinate: POST /api/mcp/audit/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; module:VirtoCommerce.UCP=3.1011.0-pr-13-5de2; store=B2B-store
    at: 2026-11-10T14:20:56.415Z
    by: session:40688c0a
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; module:VirtoCommerce.UCP=3.1011.0-pr-13-5de2; store=B2B-store
    at: 2026-11-10T14:55:30.072Z
    by: session:40688c0a
    bodyHash: c704fe9c462f
    who: Lenajava1
    note: "Re-observed 2026-11-10 ~14:55Z with a fresh pair of PKCE tokens (buyers in two organizations): foreign cart id via flat get_cart -> raw XCart 'Access denied.' Forbidden envelope; via standard get_cart -> xapi_execution_failed (recoverable); a random unknown cart id -> cart_not_found 404 (flat) / not_found unrecoverable (standard). So a foreign cart is distinguishable from an unknown one (existence observable), while orders are not (foreign and unknown both order_not_found / not_found). No line, total or address of the foreign cart was returned; list_carts did not include it."
---
2026-11-10, two B2B buyers in different organizations, each with an authorization-code + PKCE token for client claude-code-ucp-qa (aud {FRONT_URL}/ucp/mcp). With buyer B's token on buyer A's data: flat get_cart / update_cart / checkout_and_handoff by cart_id -> isError with the raw XCart GraphQL envelope {"errors":[{"message":"Access denied.","path":["cart"],"extensions":{"code":"Forbidden"}}],"data":{"cart":null}} (not a normalized UCP error code); the standard get_cart / get_checkout by id -> UCP error xapi_execution_failed severity recoverable; any call passing buyer A's buyer_id -> buyer_context_mismatch 403; track_order by order_number, order_id or cart_id+placed_after and standard get_order -> order_not_found 404 (existence hidden). Anonymous calls carrying buyer A's real buyer_id -> buyer_context_mismatch 403 'Anonymous buyer context is invalid.'. Nothing was changed on A's cart. The audit log (api/mcp/audit/search, scopeName ucp) records each refusal with B's user and the client, outcome Failed.
