---
id: KB-12BED7EA
subject: With Back-in-Stock enabled (theme 2.59), an out-of-stock product page replaces the quantity stepper with a 'Stock alert' button and shows no 'Out of stock' label
plane: experiential
question: How does the product page indicate an out-of-stock product?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: 2.59.0
anchors:
  - coordinate: Query.product
  - coordinate: /account/back-in-stock
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T06:46:55.635Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T21:25:56.598Z
    by: session:624d35a1
    who: kutasinaelena
    note: "QA-OOS-001 PDP signed in: 'Stock alert' button, price $19.99, no stepper, no In stock / Out of stock label."
---
Signed-in buyer, theme 2.59.0, product qty 0: 'Price and delivery' shows the price and a 'Stock alert' (crossed-bell) subscribe button in place of the quantity stepper; no 'Out of stock' text. Catalog cards show 'Stock alert' + 'Click to receive an alert'. On a theme 2.51.2 store (anonymous) the out-of-stock product page shows a disabled stepper and an 'Out of stock 0' badge instead. Tests that require an explicit 'Out of stock' label must accept the Stock alert CTA when back-in-stock is on.
