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
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    at: 2026-10-15T09:28:21.151Z
    by: session:10db9d7d
    bodyHash: fe3b4000528c
    who: Lenajava1
    note: "QA-OOS-001 PDP signed in as B2B org admin on vcst_qa, Ver. 2.60.0-alpha.2579: price $19.99, 'Stock alert' button replaces stepper, no Out of stock text."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    at: 2026-10-28T10:46:59.052Z
    by: session:7e8ea7fb
    bodyHash: fe3b4000528c
    who: Lenajava1
    note: "Re-observed on theme 2.61.0-alpha.2605: QA-OOS-001 PDP signed in as personal user shows 'Stock alert' button (no stepper); after click it reads \"You'll be notified\"; clicking it again toggles back to 'Stock alert'."
---
Signed-in buyer, theme 2.59.0, product qty 0: 'Price and delivery' shows the price and a 'Stock alert' (crossed-bell) subscribe button in place of the quantity stepper; no 'Out of stock' text. Catalog cards show 'Stock alert' + 'Click to receive an alert'. On a theme 2.51.2 store (anonymous) the out-of-stock product page shows a disabled stepper and an 'Out of stock 0' badge instead. Tests that require an explicit 'Out of stock' label must accept the Stock alert CTA when back-in-stock is on.
