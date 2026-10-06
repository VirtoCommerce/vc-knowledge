---
id: KB-A4ABEFEA
subject: Notification journal search returns full email bodies; keyword matches the recipient
plane: experiential
question: How do I read the sent email body for a recipient from POST /api/notifications/journal?
questions:
  - text: How can a tester read the email that was sent to a customer?
  - text: POST /api/notifications/journal keyword recipient body
  - text: Does the notification journal search response include the email body, or is GET /api/notifications/journal/{id} needed?
  - text: What does keyword match in the notification journal search?
  - text: Is the email body stored when SMTP is not configured and the journal status is Error?
concepts:
  - id: notification
  - id: rest-api
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/notifications/journal
  - coordinate: GET /api/notifications/journal/{id}
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-09-30T10:44:52.956Z
    by: session:p22116
    who: Aleksandra-Mitricheva
---
POST /api/notifications/journal with {notificationType, keyword: <recipient email>, sort: 'createdDate:desc'} returns EmailNotificationMessage rows that already include from, to, cc, subject, body and tenantIdentity, so GET /api/notifications/journal/{id} is not needed for the body. keyword matched the to field (all hits had that recipient when combined with notificationType); keyword alone matched far more rows, so filter to client-side. On a deployment without SMTP the row is stored with status Error but the rendered body is present. to can hold several recipients separated by '; '.
