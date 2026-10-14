---
id: KB-EC0D1A9B
subject: "A store with no fulfillment center cannot take any product into the cart: addItem fails with PRODUCT_FFC_QTY"
plane: experiential
question: Why does addItem add nothing in a store whose products show isBuyable true but isAvailable false?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.xcart
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.addItem
  - coordinate: Query.products
  - coordinate: GET /api/stores/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-14T07:33:46.328Z
    by: session:cae69cc8
    who: kutasinaelena
---
On store New-super (mainFulfillmentCenterId null, no additional fulfillment centers; same catalog as B2B-store) xAPI products returned 66 products with availabilityData isBuyable=true but isAvailable=false, isInStock=false, availableQuantity=0 for all of them. In B2B-store, which has fulfillment centers, the same buyer could add stocked products. addItem for a product with availableQuantity 0 returns the cart unchanged with validationErrors [{errorCode: PRODUCT_FFC_QTY, 'Product with Id … was not added to cart. Available quantity is 0.'}] and no GraphQL error. isBuyable alone therefore does not mean the product can be ordered. Filter on isAvailable && availableQuantity >= qty, and read validationErrors after addItem.
