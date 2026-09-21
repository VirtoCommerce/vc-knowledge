---
id: KB-F78ED1CC
subject: On vcst-qa a configurable product's PDP has a real Add to cart button, while simple and variation PDPs add via the quantity stepper.
plane: experiential
question: what is the add-to-cart control on a storefront product page, and does it differ by product type
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /products-with-options/custom-t-shirt
  - coordinate: /soft-drinks/soda
  - coordinate: Query.product
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T10:58:49.086Z
    by: session:local_de
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:02:59.189Z
    by: session:local_e8
    note: Re-observed on AGENT-TEST-CFG-Bike-Flat (/products-with-options/cfg-parents/agent-test-cfg-032). The PDP sidebar "Price and delivery" card carries a real "Add to cart" button (data-test-id absent; resolved by role+name) and no quantity stepper at all — the parent quantity is implicitly 1. Adding fired Mutation.addItem with quantity:1.
---
Two different add controls coexist on vcst-qa (B2B-store, Ver. 2.58.0-pr-2467), chosen by product type. A CONFIGURABLE product's PDP (e.g. /products-with-options/configurable-caps-shirts/custom-t-shirt) shows an explicit 'Add to cart' BUTTON in the Price and delivery block, and clicking it appends ?lineItemId=<guid> to the URL. A SIMPLE product's PDP and a VARIATION's own PDP show NO button at all: the control is the quantity stepper, and clicking 'Increase quantity' from 0 to 1 is what creates the cart line — confirmed by an 'in Cart: 1' badge appearing next to the 'In stock' badge. This CONTRADICTS KB-0C163966, which recorded on vcptcore_stable that 'this storefront has no Add-to-cart button for any product, configurable or not'. The stepper half of that claim holds here; the configurable half does not, so the add control is deployment/theme-version dependent and must not be assumed from one environment. A MASTER product's PDP shows a third shape: price 'N/A' and a DISABLED button labelled 'Add to cart' with accessible name 'Select options to proceed', pending variant selection. Category-card shapes mirror this and are a reliable product-type signal in a listing: 'Customize' link = configurable, 'N variations' + 'From $X' = master, bare price + stepper + In stock = directly purchasable.
