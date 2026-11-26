---
id: KB-EB8C74EC
subject: List-page 'Add all to cart' with an out-of-stock item adds the in-stock lines and reports the OOS one as 'Not added' in the result dialog
plane: experiential
question: What happens when 'Add all to cart' runs on a list that contains an out-of-stock product?
questions:
  - text: Does Add all to cart skip out-of-stock list items?
  - text: Does an out-of-stock item block adding a whole list to the cart?
  - text: What does the Adding products to cart result dialog show for an OOS list item?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists/{id}
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; store=B2B-store; role=personal customer
    at: 2026-11-26T07:09:11.514Z
    by: session:779b94fc
    who: kutasinaelena
---
Personal customer, empty cart, list of 3 in-stock products plus one product with stock 0 (its list row shows 'Out of stock', a disabled Add to cart and 'The product is no longer available for purchase'). Clicking 'Add all to cart' on /account/lists/{id} opened 'Adding products to cart result' with 'Successfully added 3' and 'Not added 1'; /cart then held only the 3 in-stock lines with the list quantities (4/3/1). Add all to cart is additive: a SKU already in the cart has the list quantity added to it. List rows stay in the list after adding.
