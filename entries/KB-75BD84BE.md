---
id: KB-75BD84BE
subject: UCP cart projection omits promotion gift lines that exist in the platform cart
plane: experiential
question: "UCP MCP get_cart: are promotion gift items (isGift) shown in line_items?"
questions:
  - text: Why does the AI shopping assistant not show the free gift that my promotion added to the cart?
  - text: Does the agent commerce cart projection include auto-added promotional gift lines?
  - text: The platform cart has an isGift line at zero price but the MCP get_cart line_items do not - is that expected?
  - text: Are gift rewards missing from the agent-facing create cart, get cart and checkout line items?
concepts:
  - id: ucp
  - id: gift
status: active
appliesTo:
  - axis: surface
    value: ucp
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /ucp/mcp
  - coordinate: GET /api/carts/{id}
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T12:10:52.543Z
    by: session:p38136
    who: Lenajava1
---
On UCP 3.1007.0-pr-9-91ca, when a store promotion auto-adds a gift reward to an anonymous cart built via create_cart, the platform cart (GET /api/carts/{id}) holds an extra isGift=true line at price 0, but UCP create_cart/get_cart/checkout line_items do not list it.
