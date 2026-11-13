---
id: KB-CA3C6EB1
subject: "onX create-sales-order by SKU fails with a raw SQL error: the line item is saved without CatalogId"
plane: experiential
question: What does onX MCP create-sales-order return for an order with a customer and a SKU line on vcst_qa?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/order/customerOrders
evidence:
  - method: observation
    deployment: vcst_qa
    build: b0d1ac6487d06555
    conditions: platform=3.1097.0; store=B2B-store
    defect: suspected
    at: 2026-11-13T12:50:31.881Z
    by: session:c625fcfd
    who: Lenajava1
---
2026-11-13, vcst_qa, admin token from the vc-mcp-onx flow, store B2B-store. create-sales-order with order.customer {id, createdAt, updatedAt, tenantId} and lineItems [{sku, quantity:1}] returns success:false, error.code INTERNAL_SERVER_ERROR, message "Cannot insert the value NULL into column 'CatalogId', table '<database>.dbo.OrderLineItem'", i.e. the SQL error text, including the database name, reaches the MCP client. Same with an inactive (566903892) and an active (564698896) product. Before that the tool's input schema requires customer.createdAt/updatedAt/tenantId, while its own error text asks only for customer.id or customer.externalId. The onX docs list create-sales-order as a supported operation.
