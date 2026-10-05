---
id: KB-E4DE692A
subject: UCP create_cart/update_cart reject duplicate quantities whose sum overflows int32 as invalid_request
plane: experiential
question: "UCP MCP create_cart or update_cart duplicate line quantities summing above int32 max: what error?"
questions:
  - text: What happens if a shopping agent adds the same item twice with quantities that add up to an absurdly large number?
  - text: How does the agent cart API respond when duplicate line quantities for one product overflow a 32-bit integer?
  - text: Is the existing cart changed when an update with an overflowing combined quantity is rejected?
  - text: Which error code and HTTP status do MCP and REST cart calls return for an out-of-range combined line quantity?
concepts:
  - id: ucp
  - id: cart-validation
  - id: api-error
status: active
appliesTo:
  - axis: surface
    value: ucp
anchors:
  - coordinate: POST /ucp/v1/carts
  - coordinate: PUT /ucp/v1/carts/{id}
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T12:10:51.962Z
    by: session:p62420
    who: Lenajava1
---
On UCP 3.1007.0-pr-9-91ca duplicate entries 2147483647 + 1 for one product return MCP isError true, code invalid_request, status_code 400, message 'The combined line item quantity is out of range.', empty details; REST POST /ucp/v1/carts and PUT /ucp/v1/carts/{id} return HTTP 400 with the same code. On update_cart the existing cart is unchanged (line stays at its prior quantity).
