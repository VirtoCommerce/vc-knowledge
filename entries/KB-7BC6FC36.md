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
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: theme=2.62.0-pr-2628-d9ae-d9aeda83; store=B2B-store; role=personal account
    at: 2026-11-11T14:06:36.282Z
    by: session:7773ae1e
    bodyHash: 7d17b2f3b058
    who: Lenajava1
    note: "Hybrid stand (local PR 2628 theme + vcst-qa backend), Firefox: USD->EUR on USD-only waffle-maker PDP shows Price EUR 0.00, disabled stepper, in-stock badge, no unavailability message. Same as entry; case CAT-054 asserts the opposite."
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; store=B2B-store; role=personal account
    at: 2026-11-11T19:10:20.464Z
    by: session:7e403256
    bodyHash: 7d17b2f3b058
    defect: VCST-6605
    who: kutasinaelena
    note: "Chrome, theme 2.62.0-alpha.2647: USD->EUR on USD-only AGENT-TEST-Gaming Laptop PDP shows Price EUR 0.00, disabled stepper, In stock 9, no unavailability message. Still present."
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store; currency=EUR; culture=de-DE; viewport=390x844
    at: 2026-11-16T16:49:27.526Z
    by: session:d9b8e380
    bodyHash: 7d17b2f3b058
    who: kutasinaelena
    note: "Theme 2.63.0-pr-2602, EUR account (no currency switch, account default EUR), 390 px: PEPSI 201482 (USD-only) PDP 'Preis: 0,00 €', stepper 0 disabled, in-stock badge 3529, no unavailability message; GetProduct EUR isBuyable=false. Additionally the new Subscribe & Save block renders with 'N/A' (KB-4745333B)."
  - method: observation
    deployment: vcst_qa
    build: b8b4b94cd1e4180f
    conditions: theme=2.63.0-pr-2630-a18a-a18a3bb2; store=B2B-store; role=personal account
    at: 2026-11-19T19:15:34.791Z
    by: session:51ac2fe3
    bodyHash: 7d17b2f3b058
    who: Lenajava1
    note: "REG-2026-11-19-1857 chrome, flag-off storefront build 2630: USD->GBP on USD-only (GBP/CZK missing) Canon PDP shows Price GBP 0.00, disabled stepper, In stock 9042, no unavailability message; related 'Customers bought together' cards GBP 0.00. CAT-054 fails."
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; platform=3.1101.0; store=B2B-store; role=personal account
    at: 2026-11-26T02:37:33.270Z
    by: session:779b94fc
    bodyHash: 7d17b2f3b058
    defect: VCST-6605
    who: kutasinaelena
    note: "Still present on theme 2.63.0-alpha.2681, Firefox, personal account: USD->EUR on USD-only PEPSI 201482 PDP shows Price €0.00, disabled stepper, In stock 906, no unavailability message; related cards €0.00."
---
Signed-in buyer, store with USD+EUR. Product priced only in USD (xAPI EUR price.actual 0, isBuyable false). Header currency -> EUR: product page 'Price and delivery' reads 'Price: €0.00', quantity stepper and input disabled, in-stock badge still shown, no message that the product is unavailable in this currency. The cart (converted to EUR) shows that line at €0.00; catalog cards show '€0.00'. A product with an EUR price shows its EUR price-list value, not a conversion.
