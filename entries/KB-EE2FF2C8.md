---
id: KB-EE2FF2C8
subject: Storefront cart line 'Price per item' follows LineItem.showPlacedPrice on desktop but always shows the placed price in the narrow (card) layout
plane: experiential
question: Does the /cart 'Price per item' show list price on desktop but placed (discounted) price on mobile?
status: active
appliesTo:
  - axis: page
    value: cart
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.cart
  - coordinate: CartType.items.showPlacedPrice
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-14T17:32:16.244Z
    by: session:cae69cc8
    who: kutasinaelena
---
On theme 2.60.0-pr-2564 the cart line component renders two price elements: a desktop column (container wider than 4xl) whose shown price is showPlacedPrice ? placedPrice : listPrice, and a 'Price per item' property used in the narrow card layout (seen at 375 and 768 px) that always shows placedPrice ?? salePrice ?? listPrice. In a regular password session (B2B-store, USD) the cart returned showPlacedPrice=true: a sale-priced line (list 400.00, sale 237.00, qty 2) showed 237.00 with 400.00 struck at 1920 px and 237.00 at 375/768 px, line total 474.00 at all widths, so desktop and mobile agree. When showPlacedPrice is false (observed for the same buyer in a punchout session), desktop shows the list price and the struck list total, while the narrow layout still shows the placed price, so the two layouts disagree.
