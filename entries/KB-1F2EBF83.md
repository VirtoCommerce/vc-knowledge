---
id: KB-1F2EBF83
subject: Legacy PUT /api/return with no id creates a return in status New, and raises no ReturnRegistered email or push
plane: experiential
question: How do I create a return as an admin via REST PUT /api/return, and does it notify the buyer?
questions:
  - text: If support opens a return for me from the back office, will I get a confirmation email?
  - text: How does an admin create a return through the REST API when there is no POST create route?
  - text: What status does a return created via PUT without an id start in, New or Requested?
  - text: Does a return created over REST reduce the order's returnable quantity?
  - text: Which path raises the return-registered notification and push -- REST upsert or the GraphQL submit?
concepts:
  - id: return
  - id: returnable-quantity
  - id: notification
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/return
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T21:35:06.798Z
    by: session:p10400
    who: kutasinaelena
---
The Return module REST controller has no POST create; PUT /api/return with a body that has no id upserts a new return (200, returns the created id; number from Return.ReturnNewNumberTemplate). Body used: storeId, customerId, customerName, orderId, orderNumber, customerReference, status New, lineItems[] with orderLineItemId, productId, sku, name, measureUnit, orderedQuantity, price, quantity, reason. The return lands in status New (not Requested), it DOES hold its quantity against returnableItems (ordered 4, returned 3 -> returnableQuantity 1), languageCode is filled from the order (en-US), and NO ReturnRegisteredEmailNotification and NO push message are raised - those come only from xAPI submitReturn. Observed on VirtoCommerce.Return 3.1003.0-pr-27-51fc.
