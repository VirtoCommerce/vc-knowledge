---
id: KB-54D8DEDA
subject: "Platform REST cart add-item does not evaluate prices: the line item keeps whatever listPrice the request body carries (0 when omitted)"
plane: experiential
question: "POST /api/carts/{cartId}/items REST cart: does adding a line item evaluate the price from the pricing module, or is listPrice taken from the request body?"
status: active
appliesTo:
  - axis: module
    value: cart
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: POST /api/carts/{cartId}/items
  - coordinate: POST /api/pricing/evaluate
evidence:
  - method: observation + source
    deployment: vcptcore_regression
    at: 2026-10-13T15:41:33.612Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T17:55:52.846Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-observed: line item keeps the supplied listPrice 16.15 / salePrice 15; after quantity=5 the cart extendedPrice/subTotal/total = 80.75 = 5 x listPrice (placedPrice defaults to listPrice, sale price not used in totals when placedPrice is not supplied)."
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:52:59.561Z
    by: session:624d35a1
    who: kutasinaelena
---
On Stable 16 (Cart 3.1012.0) a line item added via POST /api/carts/{cartId}/items without price fields came back with listPrice=0 while POST /api/pricing/evaluate for the same product returned list=200. Cart module source (CartModuleController.AddItemToCart -> ShoppingCartBuilder.AddItem(lineItem)) takes the LineItem from the body and does not call the pricing service; price evaluation for carts lives in xAPI (XCart addItem). Totals are then computed from the client-supplied price. A REST-cart test that expects pricing-module prices must pass the price itself.
