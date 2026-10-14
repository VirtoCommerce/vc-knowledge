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
---
Signed-in buyer, store with USD+EUR. Product priced only in USD (xAPI EUR price.actual 0, isBuyable false). Header currency -> EUR: product page 'Price and delivery' reads 'Price: €0.00', quantity stepper and input disabled, in-stock badge still shown, no message that the product is unavailable in this currency. The cart (converted to EUR) shows that line at €0.00; catalog cards show '€0.00'. A product with an EUR price shows its EUR price-list value, not a conversion.
