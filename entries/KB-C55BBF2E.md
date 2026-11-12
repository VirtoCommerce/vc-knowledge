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
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2647; module:VirtoCommerce.XCart=3.1046.0; module:VirtoCommerce.Xapi=3.1031.0; store=B2B-store; role=personal customer
    at: 2026-11-12T10:50:17.281Z
    by: session:517b1f88
    bodyHash: 49f5a83a63dc
    who: Lenajava1
    note: "Four-line cart after a guest-to-account merge: subTotal 514 = list 150 + 150 + 25 + 189; discountTotal 87 = (150-106) + (150-107) from two sale-priced lines; the lines showed actual 106 / 107 and the /cart Subtotal label showed $514.00."
---
A sale price is modelled as the list price in the cart subtotal plus a line-level sale discount, not as a lower subtotal: the order summary subtotal shows the list price, the line's placed and extended price show the sale price, and the difference appears as a line discount. The cart's aggregate discountTotal therefore bundles sale and coupon discounts, and a coupon's own effect is its entry in the addCoupon response's discounts. Because tax is recomputed on the discounted base, the grand-total change does not equal the discount; subtotal minus discount plus tax plus shipping equals the grand total.
