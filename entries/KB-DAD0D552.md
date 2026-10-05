---
id: KB-DAD0D552
subject: UCP refuses over-stock cart and checkout calls with isError, a stable inventory code and structured quantities
plane: experiential
question: What does UCP /ucp/mcp create_cart, update_cart or create_checkout return when the requested quantity exceeds available stock?
questions:
  - text: What does a shopping agent get back when it asks for more units than are in stock?
  - text: Which error codes distinguish partial stock, zero stock and an unmeetable minimum order quantity for agent cart calls?
  - text: Does a refused over-stock cart update roll back, or can the over-stock quantity persist on the line?
  - text: Do checkout and handoff tools refuse a cart with inventory errors, and is a continue_url returned?
  - text: What fields are in the details of an MCP inventory refusal, and which codes are retryable?
  - text: If an agent tries to put more items than are in stock in my cart, what quantity stays in it?
  - text: Is a refused cart update over stock atomic, or does the over-stock quantity persist?
  - text: After update_cart fails with insufficient_stock, what does get_cart report per line?
  - text: Why does checkout handoff give no continue URL after an out-of-stock cart update?
concepts:
  - id: ucp
  - id: inventory
  - id: api-error
  - id: cart-validation
status: active
appliesTo:
  - axis: surface
    value: ucp
anchors:
  - coordinate: /ucp/mcp
  - coordinate: POST /ucp/v1/carts
  - coordinate: PUT /ucp/v1/carts/{cartId}
  - coordinate: POST /ucp/v1/checkouts
  - coordinate: POST /ucp/v1/checkouts/{checkoutId}/handoff
  - coordinate: POST /ucp/v1/internal/handoff/restore
  - coordinate: POST /ucp/mcp
  - coordinate: POST /ucp/v1/checkouts/{id}/handoff
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-29T11:45:18.076Z
    by: session:p10252
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-29T12:10:51.392Z
    by: session:p54992
    who: Lenajava1
    mergedFrom: KB-B142F52E
---
On the UCP build carrying the inventory-error normalization, a cart mutation or checkout call whose quantity exceeds stock returns an MCP tool error (result.isError true, structuredContent.is_error true, status_code 409) with code insufficient_stock when some stock exists, out_of_stock when available is 0, or inventory_unavailable when the minimum order quantity cannot be met from current stock. details carries product_id, requested_quantity, available_quantity (omitted for inventory_unavailable), retryable (true for insufficient_stock, false for the other two), cart_id, operation_rejected true, line_item_id when the line already existed, and a nested errors[] per offending line. The REST routes return HTTP 409 with the same body. create_checkout, update_checkout, checkout_and_handoff, handoff_checkout, the REST checkout handoff route and the internal handoff restore route all refuse such a cart with the same code and no continue_url. The refusal is not a rollback: update_cart can persist the over-stock quantity on an existing line, and get_cart then reports it honestly through cart.inventory_errors and per-line requested_quantity, available_quantity and inventory_status. Observed on UCP 3.1007.0-pr-9-91ca: an update_cart with duplicate entries 4+5 against stock 5 was refused with insufficient_stock (the same error listed twice in details.errors[]), yet get_cart afterwards returned ucp.status success with the line at quantity 9, requested_quantity 9 / available_quantity 5 / inventory_status insufficient_stock, cart.inventory_errors populated and cart messages[] empty; correcting the line to an in-stock quantity cleared inventory_errors and checkout_and_handoff then succeeded. A refused create_cart for a new anonymous buyer leaves an empty cart and its details do not include buyer_id.
