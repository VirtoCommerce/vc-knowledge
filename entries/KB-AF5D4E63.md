---
id: KB-AF5D4E63
subject: On virtostart, product UGR-75774 reports availabilityData.isInStock=true / isBuyable=true with availableQuantity 0, because one of four fulfillment centers (vs-dc-east) has allowBackorder=true while every FFC holds inStockQuantity 0
plane: experiential
question: Why does Query.products say a product is in stock when every fulfillment center has zero quantity?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: virtostart
    build: 8f3b3fdd36b67c83
    conditions: theme=3.3.0; store=B2B-store
    at: 2026-11-17T13:09:14.738Z
    by: session:5a497f21
    who: Lenajava1
---
Query.products (storeId B2B-store, query UGR-75774) returned isTrackInventory true, isInStock true, isBuyable true, availableQuantity 0; inventories: vs-dist-voltbridge, vs-dist-arcline, vs-dc-west (all 0, no backorder) and vs-dc-east (0, allowBackorder true). The catalog availability treats backorder on any FFC as in stock; the storefront shows 'In stock' without a count while the cart reports the line out of stock, so cart validation evidently judges a different FFC/quantity rule. Cart side (GetFullCart validationErrors) not captured in this session.
