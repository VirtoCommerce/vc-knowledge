---
id: KB-CF6239FA
subject: Storefront order detail line items show image, name link, properties, price per item, quantity and total, but no SKU
plane: experiential
question: Does /account/orders/{id} show the SKU of each ordered line item?
questions:
  - text: Is the product code visible per line on the storefront order details page?
  - text: What columns does the order detail line item list render?
status: active
appliesTo:
  - axis: feature
    value: order-details
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders/{id}
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 5609fc0c36e7302c
    conditions: platform=3.1076.7; theme=2.59.0; store=B2B-store
    at: 2026-10-21T13:43:12.743Z
    by: session:624d35a1
    who: kutasinaelena
---
Theme 2.59.0, signed in as an org buyer, opened a 2-line order from /account/orders. Each line rendered the product image, the product name as a link, product properties, Price per item, a disabled Product quantity spinbutton and the line Total; no SKU/code text appears on any line. Source 2.59.0: order-line-items.vue passes with-image/with-properties/with-price/with-total to VcLineItems, and prepareLineItems carries sku only into a data-product-sku attribute, not visible text. The StorefrontUserGuide Orders page lists items grouped by vendor, prices per item and totals, and does not promise a SKU. So a case asserting a visible SKU per order line tests something the shipped storefront never renders.
