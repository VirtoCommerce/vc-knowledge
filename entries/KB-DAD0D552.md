---
id: KB-DAD0D552
subject: UCP refuses over-stock cart and checkout calls with isError, a stable inventory code and structured quantities
plane: experiential
question: What does UCP /ucp/mcp create_cart, update_cart or create_checkout return when the requested quantity exceeds available stock?
status: active
appliesTo:
  - axis: surface
    value: ucp-mcp
anchors:
  - coordinate: /ucp/mcp
  - coordinate: POST /ucp/v1/carts
  - coordinate: PUT /ucp/v1/carts/{cartId}
  - coordinate: POST /ucp/v1/checkouts
  - coordinate: POST /ucp/v1/checkouts/{checkoutId}/handoff
  - coordinate: POST /ucp/v1/internal/handoff/restore
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-29T11:45:18.076Z
    by: session:p10252
    who: Lenajava1
---
On the UCP build carrying the inventory-error normalization, a cart mutation or checkout call whose quantity exceeds stock returns an MCP tool error (result.isError true, structuredContent.is_error true, status_code 409) with code insufficient_stock when some stock exists, out_of_stock when available is 0, or inventory_unavailable when the minimum order quantity cannot be met from current stock. details carries product_id, requested_quantity, available_quantity (omitted for inventory_unavailable), retryable (true for insufficient_stock, false for the other two), cart_id, operation_rejected true, line_item_id when the line already existed, and a nested errors[] per offending line. The REST routes return HTTP 409 with the same body. create_checkout, update_checkout, checkout_and_handoff, handoff_checkout, the REST checkout handoff route and the internal handoff restore route all refuse such a cart with the same code and no continue_url. The refusal is not a rollback: update_cart can persist the over-stock quantity on an existing line, and get_cart then reports it honestly through cart.inventory_errors and per-line requested_quantity, available_quantity and inventory_status. A refused create_cart for a new anonymous buyer leaves an empty cart and its details do not include buyer_id.
