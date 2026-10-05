---
id: KB-9B9FAF0F
subject: submitting a return raises one registered-return email notification and one push message whose topic is the return number
plane: experiential
question: What notifications does submitting a return produce, and how do I find them in the notification journal and push message search?
questions:
  - text: After I submit a return request, what messages should I receive?
  - text: How do I verify in the back office that a return submission raised its email and in-app notification?
  - text: Which notification types and push texts are raised when a return is approved partly or declined?
  - text: Why is the registered-return email journal row in Error status, and does that mean it was not raised?
  - text: How is a return's push message identified in the push message search results?
concepts:
  - id: return
  - id: notification
  - id: push-message
status: superseded
supersededBy: KB-17555498
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: rest
anchors:
  - coordinate: Mutation.submitReturn
  - coordinate: POST /api/notifications/journal
  - coordinate: POST /api/push-message/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T21:35:09.766Z
    by: session:p14880
    who: kutasinaelena
    splitFrom: KB-1564BA07
---
Each xAPI submitReturn (Draft -> Requested) produced exactly one ReturnRegisteredEmailNotification row in POST /api/notifications/journal with tenantIdentity {id: returnId, type: Return} - status Error on a deployment without SMTP (SentNotificationException), which still proves it was raised - and exactly one push message in POST /api/push-message/search with topic = the return number, shortMessage 'Return <number> received', memberIds = [the buyer contact id], status Sent. Decisions raise ReturnPartiallyApprovedEmailNotification / ReturnRejectedEmailNotification with push texts 'partly approved' / 'declined'. Observed on VirtoCommerce.Return 3.1003.0-pr-27-51fc, PushMessages 3.1006.0-pr-28-2027.
