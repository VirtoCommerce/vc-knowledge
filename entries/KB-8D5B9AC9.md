---
id: KB-8D5B9AC9
subject: productPickupLocations / cartPickupLocations ignore a keyword that contains an apostrophe (or is only '&') and return every location, while Query.pickupLocations returns 0 for the same keyword
plane: experiential
question: How do the pickup-location keyword searches treat special characters such as an apostrophe or ampersand?
status: active
appliesTo:
  - axis: feature
    value: bopis
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.productPickupLocations
  - coordinate: Query.cartPickupLocations
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T00:25:27.100Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T00:43:47.702Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-run 2026-10-15 as admin token: productPickupLocations \"O'Neil\" -> 102, pickupLocations -> 0. Root cause and other syntax-character cases are in a separate entry about the keyword phrase parser."
---
2026-10-15 vcptcore_regression (signed-in buyer, product with 102 eligible pickup locations). productPickupLocations(keyword): "zzqq" -> 0, "Neil Sons" -> 0, "Neil & Sons" -> 0, but "O'Neil" -> 102, "O'Neil & Sons" -> 102, "'" -> 102, "&" -> 102 (totalCount = all). The storefront cart 'Select address' pickup dialog sends the same keyword to cartPickupLocations and shows all 17 pages for "O'Neil & Sons" instead of the no-results message. Anonymous Query.pickupLocations(storeId, keyword:"'" or "&") returns 0 on both vcptcore_regression and vcptcore_stable, so only the product/cart-scoped searches drop such a keyword.
