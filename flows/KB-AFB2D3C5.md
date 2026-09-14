---
id: KB-AFB2D3C5
subject: an order placed on the storefront and read back in Admin
plane: flow
question: how do I place an order on this storefront, from finding a product to reading it back in Admin
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: /search
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:22:45.754Z
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:51:28.839Z
---

An order is placed ON THE CART PAGE. This storefront has no /checkout route: delivery method, payment method and Place order are all controls on /cart, and a run that goes looking for a checkout route does not find one. The sequence below is distilled from run 07's tool log (MEASUREMENT-archive/run-07-order-fields), which walked it on 2026-09-13 and spent 78 calls between reaching /cart and seeing the order in Admin, most of them rediscovering these steps.

STEP 1 - find a product. /search?q=<term> is the only product search the storefront offers; there is no REST route that searches by product type (see the entry anchored on /api/catalog/products). Searching a VARIATION's SKU returns its PARENT product page, which is how run 07 located one.

STEP 2 - add a simple product. On the product page the quantity stepper IS the add-to-cart control - incrementing it from zero puts the line in the cart. There is no separate Add to cart button to look for.

STEP 3 - add a variation. A parent product page lists its variations as rows, each with its own quantity stepper. Incrementing a ROW adds that variation, not the parent. The parent is itself a row in that list.

STEP 4 - check out, on /cart. Open /cart, then in order: Select a delivery method, choose one (Fixed Rate (Ground) exists on this store), Select a payment method, choose one (Test payment method exists and completes without an external provider), then Place order. Each of the first four is a control that opens a panel, so allow a snapshot between them.

STEP 5 - read the order back on the storefront. It lands at /account/orders/{id} with a human number of the form CO<yymmdd>-NNNNN.

STEP 6 - read it in Admin. Navigating to #!/orders alone does NOT open the Orders module; run 07 had to open the More menu and click Orders from it, then the order row, then the Line items widget to see per-line fields.

An order cannot be deleted once placed - only cancelled.
