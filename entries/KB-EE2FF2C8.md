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
  - method: observation
    deployment: vcptcore_qa1
    build: a065342a82b3d4c1
    at: 2026-10-20T12:13:06.580Z
    by: session:9bc86b2b
    bodyHash: d2d1e2784342
    defect: VCST-6375
    who: Aleksandra-Mitricheva
    note: "Reproduced on the released qa1 theme in a regular password session with showPlacedPrice=false caused by an item promotion (no punchout): wide column 180/400/89 (list), narrow slot at 1280 (container 835px) and 390 showed 144/189.60/71.20 (placed). The vc-frontend PR #2588 build (fe-f23723b99db0) shows list in the narrow slot too."
---
On theme 2.60.0-pr-2564 the cart line component renders two price elements: a desktop column (container wider than 4xl) whose shown price is showPlacedPrice ? placedPrice : listPrice, and a 'Price per item' property used in the narrow card layout (seen at 375 and 768 px) that always shows placedPrice ?? salePrice ?? listPrice. In a regular password session (B2B-store, USD) the cart returned showPlacedPrice=true: a sale-priced line (list 400.00, sale 237.00, qty 2) showed 237.00 with 400.00 struck at 1920 px and 237.00 at 375/768 px, line total 474.00 at all widths, so desktop and mobile agree. When showPlacedPrice is false (observed for the same buyer in a punchout session), desktop shows the list price and the struck list total, while the narrow layout still shows the placed price, so the two layouts disagree.
