---
id: KB-C55BBF2E
subject: a sale price is carried as the list subtotal plus a line discount, not a reduced subtotal
plane: experiential
question: How is a sale-priced product represented in the cart's subTotal and discountTotal?
questions:
  - text: Why does my basket subtotal show the full price when the product is on sale?
  - text: "How does the cart represent a sale-priced item: reduced subtotal or list subtotal plus line discount?"
  - text: Does the cart's discountTotal combine sale-price and coupon discounts, and where is a coupon's own share?
  - text: Why doesn't the grand total drop by exactly the discount amount when a coupon is applied?
  - text: Which placed and extended price does a sale line carry relative to the list price?
concepts:
  - id: price
  - id: discount
  - id: totals
status: active
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: CartType.subTotal
  - coordinate: LineItemType.placedPrice
  - coordinate: CartType.discountTotal
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:59.694Z
    by: session:memimpor
    who: Lenajava1
---
A sale price is modelled as the list price in the cart subtotal plus a line-level sale discount, not as a lower subtotal: the order summary subtotal shows the list price, the line's placed and extended price show the sale price, and the difference appears as a line discount. The cart's aggregate discountTotal therefore bundles sale and coupon discounts, and a coupon's own effect is its entry in the addCoupon response's discounts. Because tax is recomputed on the discounted base, the grand-total change does not equal the discount; subtotal minus discount plus tax plus shipping equals the grand total.
