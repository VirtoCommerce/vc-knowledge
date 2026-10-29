---
id: KB-AD392FC5
subject: A product added to the cart as a new line from the search-results quantity stepper is persisted with list/sale/placed price 0; a later priced save (e.g. AddOrUpdateCartPayment) fixes it
plane: experiential
question: Why does the stored cart (REST) hold 0 line prices while Query.cart shows catalog prices?
questions:
  - text: When does Mutation.changeCartItemsQuantity / UpdateShortCartItemQuantity save a cart line without prices?
  - text: Why does /cart sometimes re-save the cart on page load?
  - text: Which cart save repairs zero persisted line prices?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.xcart
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /api/carts/search
  - coordinate: Mutation.changeCartItemsQuantity
  - coordinate: Mutation.addOrUpdateCartPayment
  - coordinate: Mutation.createQuoteFromCart
  - coordinate: /search
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605; module:VirtoCommerce.XCart=3.1042.0; module:VirtoCommerce.Cart=3.1013.0; module:VirtoCommerce.Quote=3.1005.0
    defect: suspected
    at: 2026-10-29T10:04:05.630Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
    note: "distinct: KB-303CC514 records that the quote copies persisted prices and that they can be 0 (root cause not isolated). This entry records WHICH cart writes persist 0 (new line from the /search stepper) and which re-price (existing-line update, AddOrUpdateCartPayment on /cart load only when no payment exists)."
---
On vcst (2026-10-29) a B2B buyer clicked + on search results (/search?q=...) for two products not yet in the cart; the storefront sent UpdateShortCartItemQuantity. POST /api/carts/search (responseGroup Full) then showed both new lines with listPrice/salePrice/placedPrice 0, while Query.cart priced them (49.99, 79.99). Clicking + on a product ALREADY in the cart re-saved that line with its catalog price. Opening /cart fires Mutation AddOrUpdateCartPayment (empty payment) on load only when the cart has no payment yet; after it, the persisted lines read 49.99/79.99. When the cart already had a payment, /cart fired no save and a line added new from /search stayed at 0. createQuoteFromCart copies these persisted prices, so a quote gets 0.00 for any line added new since the last priced save, with or without a promotion gift line (reproduced with the auto gift rejected). The persisted 0 is invisible on the storefront cart.
