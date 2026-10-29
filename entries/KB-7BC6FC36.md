---
id: KB-7BC6FC36
subject: "On a product page in a currency the product has no price for, theme 2.59 shows 'Price: €0.00' with a disabled quantity stepper and no 'not available' message"
plane: experiential
question: What does the product page show after switching to a currency in which the product has no price?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: 2.59.0
anchors:
  - coordinate: Query.product
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T06:46:55.392Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b; platform=3.1086.0-pr-3139-5d63; setting:store=B2B-store
    at: 2026-10-20T14:46:24.273Z
    by: session:d992339e
    bodyHash: 7d17b2f3b058
    who: Lenajava1
    note: "Cart on theme 3.1.0-pr-11-d67b (vc-frontend 2.60.0 merge): after USD->EUR switch, USD-only lines (Coca Cola, ALCE0128) show EUR 0.00 with disabled quantity stepper AND an inline warning 'The product is no longer available for purchase' on the cart line; Place order disabled; subtotal excludes them. QA-MULTICUR-001 shows the EUR price-list value 22.49, not a conversion of 24.99."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    at: 2026-10-28T11:25:43.471Z
    by: session:7e8ea7fb
    bodyHash: 7d17b2f3b058
    who: Lenajava1
    note: "Theme 2.61.0-alpha.2605: USD->EUR on Birra Moretti 24x33cl Can PDP (USD-only) shows 'Price: €0.00', disabled stepper, no unavailability message; related-product cards also €0.00. Product with EUR price (AGENT-TEST-MultiCurrency-Fixture) shows EUR 22.49 vs USD 24.99 (price-list values). Suite case CAT-054 asserts the opposite (no zero placeholder), so it fails against this behaviour."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; platform=3.1090.0; store=B2B-store; role=personal account
    at: 2026-10-28T20:43:33.596Z
    by: session:ccf68ec7
    bodyHash: 7d17b2f3b058
    defect: VCST-6605
    who: Lenajava1
    note: "Re-observed in Firefox: USD->EUR on the USD-only PDP shows Price EUR 0.00, disabled stepper, in-stock badge, no unavailability message. Query.product (currencyCode EUR) returns price.actual.amount 0 with availabilityData.isAvailable=false, isBuyable=false, isInStock=true, so the storefront ignores the availability flags. Filed as VCST-6605."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; store=B2B-store; role=personal account
    at: 2026-10-29T00:21:44.736Z
    by: session:ce3cfc2a
    bodyHash: 7d17b2f3b058
    who: Aleksandra-Mitricheva
    note: "REG-2026-10-28-1915 chrome: USD->EUR on a USD-only fixture PDP shows Price EUR 0.00, disabled stepper, in-stock badge, no unavailability message (suite case CAT-054)."
---
Signed-in buyer, store with USD+EUR. Product priced only in USD (xAPI EUR price.actual 0, isBuyable false). Header currency -> EUR: product page 'Price and delivery' reads 'Price: €0.00', quantity stepper and input disabled, in-stock badge still shown, no message that the product is unavailable in this currency. The cart (converted to EUR) shows that line at €0.00; catalog cards show '€0.00'. A product with an EUR price shows its EUR price-list value, not a conversion.
