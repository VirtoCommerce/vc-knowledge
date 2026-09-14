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
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T08:40:17.485Z
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:36:19.798Z
---

An order is placed ON THE CART PAGE. This storefront has no /checkout route: delivery method, payment method and Place order are all controls on /cart, and a run that goes looking for a checkout route does not find one. The sequence below is distilled from run 07's tool log (MEASUREMENT-archive/run-07-order-fields), which walked it on 2026-09-13 and spent 78 calls between reaching /cart and seeing the order in Admin, most of them rediscovering these steps.

STEP 1 - find a product. /search?q=<term> is the only product search the storefront offers; there is no REST route that searches by product type (see the entry anchored on /api/catalog/products). Searching a VARIATION's SKU returns its PARENT product page, which is how run 07 located one.

STEP 2 - add a simple product. On the product page the quantity stepper IS the add-to-cart control - incrementing it from zero puts the line in the cart. There is no separate Add to cart button to look for.

STEP 3 - add a variation. A parent product page lists its variations as rows, each with its own quantity stepper. Incrementing a ROW adds that variation, not the parent. The parent is itself a row in that list.

STEP 4 - check out, on /cart. Open /cart, then in order: Select a delivery method, choose one (Fixed Rate (Ground) exists on this store), Select a payment method, choose one (Test payment method exists and completes without an external provider), then Place order. Each of the first four is a control that opens a panel, so allow a snapshot between them.

STEP 5 - read the order back on the storefront. It lands at /account/orders/{id} with a human number of the form CO<yymmdd>-NNNNN.

STEP 6 - read it in Admin. Navigating to #!/orders alone does NOT open the Orders module; run 07 had to open the More menu and click Orders from it, then the order row, then the Line items widget to see per-line fields.

An order cannot be deleted once placed - only cancelled.

## Amendments

Corrections to individual steps, each from somebody who walked this and found it wanting.
The steps above are as first written; read these with them.

- **Step 4** — Selecting a DELIVERY METHOD needs a shipping address on the cart first. Run 08's account had a default address on its organization and the field auto-populated, so this step worked without it being mentioned; an account with no default will find the delivery panel refuses to complete.  
  _observed vcptcore_stable, platform 3.1007.26 · 2026-09-14T09:15:48.829Z_
- **Step 4** — This store offers TWO Fixed Rate options, Ground and Air, not the one named here. Both cost 0.00 because neither Rate setting is configured -- see @kb(KB-6AA0D7FB) -- so the choice is recorded but priceless.  
  _observed vcptcore_stable, platform 3.1007.26 · 2026-09-14T09:15:48.949Z_
- **Step 4** — The claim that there is no /checkout route is true of checkout ITSELF and not of the whole prefix: /checkout/completed exists as the post-placement landing page. Run 09 observed it. No step depends on this, which is why run 09 confirmed the flow rather than disputing it.  
  _observed vcptcore_stable, platform 3.1007.26 · 2026-09-14T09:15:49.071Z_
- **Step 6** — The Orders module DOES have a working deep link: #!/workspace/orders opens the Customer orders blade directly, no More menu needed. The step is right that a bare #!/orders does not. From there, a CONFIGURABLE line needs one more hop than this step describes: the Line items grid shows configured and unconfigured lines identically -- same product name, same SKU, same Qty, differing only in price -- and the configuration is behind clicking the line, then its Configuration widget, then one of Configuration products / texts / files. Configuration products lists the chosen option's name AND quantity, so Admin can answer what was chosen where the storefront's own order query cannot.  
  _2026-09-14T12:36:19.936Z_
