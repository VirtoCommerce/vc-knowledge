---
id: KB-8C4BEBF2
subject: "REST setup shapes on Stable 16: POST /api/catalog/catalogs needs languages, PATCH /api/inventory/{id} takes the inventory record id, order line items need catalogId, pricing evaluate needs catalogId for a catalog-assigned price list"
plane: experiential
question: What minimal REST bodies create a catalog, initial stock, a REST customer order and an evaluated price on Stable 16?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/catalog/catalogs
  - coordinate: PATCH /api/inventory/{id}
  - coordinate: POST /api/order/customerOrders
  - coordinate: POST /api/pricing/evaluate
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:52:43.523Z
    by: session:624d35a1
    who: kutasinaelena
---
Stable 16 (platform 3.1076.1). POST /api/catalog/catalogs {name} -> 500 "Value cannot be null. (Parameter 'source')"; adding languages:[{languageCode:'en-US',isDefault:true}] -> 200. A new product has no inventory row: PATCH /api/inventory/{productId} -> 404 both before and after a row exists; create stock with PUT /api/inventory/plenty [{productId, fulfillmentCenterId, inStockQuantity}] and PATCH /api/inventory/{inventoryRecordId} (id from GET /api/inventory/products/{productId}) -> 204. POST /api/order/customerOrders needs number and customerName (400 otherwise) and each line item needs catalogId (missing -> 500 raw SQL 'Cannot insert the value NULL into column CatalogId'). POST /api/pricing/evaluate {storeId, productIds, currency, quantity} returns [] for a price list assigned to a catalog other than the store's; adding catalogId (or pricelistIds) returns the price. GET /api/products/{productId}/prices returns a flat Price array (price id at [0].id).
