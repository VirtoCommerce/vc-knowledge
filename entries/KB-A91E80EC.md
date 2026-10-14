---
id: KB-A91E80EC
subject: A pickup location is listed for a product (productPickupLocations / cartPickupLocations) only when its fulfillment center stocks that product; the store-level pickupLocations query lists every active location
plane: experiential
question: Which pickup locations do Query.cartPickupLocations / Query.productPickupLocations return compared with Query.pickupLocations?
status: active
appliesTo:
  - axis: feature
    value: bopis
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.cartPickupLocations
  - coordinate: Query.productPickupLocations
  - coordinate: Query.pickupLocations
  - coordinate: POST /api/shipping/pickup-locations
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T22:45:15.916Z
    by: session:624d35a1
    who: kutasinaelena
---
On vcptcore_regression (2026-10-14) B2B-store had 13 active pickup locations; Query.pickupLocations(storeId) returned all 13, but Query.productPickupLocations(productId) for a product whose only inventory row was on one fulfillment center returned exactly 1 location — the one whose fulfillmentCenterId was that FFC — with availabilityType Today and availableQuantity equal to that FFC's stock. After creating 101 more active locations via POST /api/shipping/pickup-locations with fulfillmentCenterId set to that same FFC, productPickupLocations returned totalCount 102 (store-level pickupLocations 114) within seconds, default order ascending by name. Source agrees: vc-module-x-pickup SearchCartPickupLocationsQueryHandler builds MultipleProductsPickupLocationSearchCriteria from the cart's selected items and passes it to ProductPickupLocationService (availability types Today / Transfer / GlobalTransfer). So a pagination fixture for the cart pickup dialog must bind its locations to an FFC that stocks the cart's product (or list it in transferFulfillmentCenterIds); the store-level count says nothing about how many a cart will see.
