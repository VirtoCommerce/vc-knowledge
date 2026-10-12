---
id: KB-4B6BF7A0
subject: xAPI wishlist items carry product.maxQuantity = 0 (not null) for a product with no maximum order quantity
plane: experiential
question: What does Query.wishlist (GetWishlist) return in items.product.maxQuantity for a product that sets no maximum order quantity?
status: active
appliesTo:
  - axis: operation
    value: getwishlist
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.wishlist
  - coordinate: /account/lists/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T11:24:58.501Z
    by: session:df9d131f
    who: Lenajava1
---
2026-10-09: created a private list on the storefront, added one in-stock product, opened /account/lists/{id}; the storefront's GetWishlist query (wishlist(listId)) returned items[0].product with minQuantity 1, maxQuantity 0, packSize 1, availabilityData.availableQuantity 1225, isInStock true. maxQuantity is a number 0, never null/absent, for a product without a maximum limit — same convention as the REST product record (KB-CE053374). A client that falls back with `??` (null-coalescing) therefore never reaches its fallback; 0 must be treated explicitly as "no limit".
