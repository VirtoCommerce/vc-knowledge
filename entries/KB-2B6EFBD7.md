---
id: KB-2B6EFBD7
subject: addItem with a quantity above available stock answers 200 with no errors[] and leaves the line out of the cart; only validationErrors(ruleSet:"*") explains it, and the empty cart then fails createOrderFromCart
plane: experiential
question: Why does createOrderFromCart say "The cart has validation errors" when Query.cart validationErrors is empty after addItem?
questions:
  - text: Does Mutation.addItem return an error when the quantity exceeds available stock?
  - text: Which validation error code does addItem report for a quantity above the fulfillment-center stock?
  - text: Why is the cart empty after addItem quantity 20 on a product with stock 3?
  - text: Which ruleSet shows ALL_LINE_ITEMS_UNSELECTED on an empty cart?
status: active
appliesTo:
  - axis: actor
    value: anonymous
  - axis: module
    value: x-cart
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Mutation.addItem
  - coordinate: Query.cart
  - coordinate: Mutation.createOrderFromCart
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; store=B2B-store; actor=anonymous
    at: 2026-11-06T10:46:14.342Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
Anonymous xAPI on store B2B-store, USD. The product had availabilityData.availableQuantity 3. Mutation.addItem with quantity 20 returned HTTP 200 with no errors[]. The returned cart had items [], and its validationErrors(ruleSet:"*") held PRODUCT_FFC_QTY ("Product with Id … was not added to cart. Available quantity is 3.", objectType CatalogProduct) and ALL_LINE_ITEMS_UNSELECTED (ShoppingCart). addItem with quantity 3 added the line. On the empty cart, a later Query.cart validationErrors with the default ruleSet, and with ruleSets items / shipments / payments, returned []. Only ruleSet "*" returned ALL_LINE_ITEMS_UNSELECTED, and PRODUCT_FFC_QTY was no longer reported at all. After a FixedRate shipment and DefaultManualPaymentMethod payment, Mutation.createOrderFromCart returned data null with the generic "The cart has validation errors" (ValidationError). The same flow with a product stocked 698 placed an order. A caller that selects only { id } from addItem, or reads the default ruleSet, cannot tell that the line was refused. Check items or validationErrors(ruleSet:"*") on the addItem response.
