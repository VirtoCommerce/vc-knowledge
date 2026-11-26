---
id: KB-4E36DFD5
subject: "Payment method rows saved with PUT /api/payment cannot be deleted through REST: DELETE /api/payment answers 405"
plane: experiential
question: Can a saved store payment method row be deleted through the REST API?
questions:
  - text: How do I remove a payment method I saved for a store via PUT /api/payment?
  - text: Is there a DELETE route in the Payment module REST API?
  - text: What happens to a store's payment method rows when a test store is torn down?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/payment
  - coordinate: DELETE /api/payment
  - coordinate: GET /api/payment/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1857bc5940dff4ff
    conditions: platform=3.1101.0; module:VirtoCommerce.Payment=3.1014.0-pr-83-9661
    at: 2026-11-26T14:31:03.086Z
    by: session:ede6a633
    who: Lenajava1
---
Saved a store payment method row with PUT /api/payment (body = the new-instance object from POST /api/payment/search plus id, storeId, isActive=false): 200, the id supplied by the client was kept. Then DELETE /api/payment?ids={id} and DELETE /api/payment/{id} both answered 405 and GET /api/payment/{id} still returned the row. The Payment module Swagger lists only GET/PUT /api/payment, POST /api/payment/search and GET /api/payment/{id}; the module source at the PR head has no store-deletion handler, so deleting the store would leave the rows behind (source reading, not observed). A test that saves payment rows cannot tear them down by REST; leave inactive rows on a throw-away store or remove them some other way.
