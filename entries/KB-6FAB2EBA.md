---
id: KB-6FAB2EBA
subject: addItem rejects a line quantity of one million or more through validationErrors, not errors
plane: experiential
question: What does addItem return when the requested quantity exceeds the store's line item limit?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.addItem
  - coordinate: CartType.validationErrors
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:14:45.082Z
    by: session:memimpor
    who: Lenajava1
---
A store-level line item limit rejects a quantity of 1,000,000 or more on addItem. The rejection appears in the cart's validationErrors inside an HTTP 200 with an empty GraphQL errors array, and the item is simply not added. A caller that checks only the HTTP status and errors[] will read the add as a success.
