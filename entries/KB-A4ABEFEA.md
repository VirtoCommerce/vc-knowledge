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
    deployment: vcptcore_qa
    at: 2026-09-30T10:44:52.956Z
    by: session:p22116
    who: Aleksandra-Mitricheva
    build: 7f4081223d65d3fa
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:53:07.364Z
    by: session:b6bd53fc
    note: POST /api/notifications/journal with objectIds + objectType Return returned to / cc / bcc and bodies for the buyer email and the organization copy.
    build: 885191398cb25816
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:55:45.507Z
    by: session:df9d131f
    who: Lenajava1
    note: POST /api/notifications/journal with keyword=<recipient email>, sort createdDate:desc returned ResetPasswordEmailNotification and OtpSignInEmailNotification rows for that recipient with status Sent and the rendered body.
    build: a6e179f178104063
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T12:28:34.037Z
    by: session:624d35a1
    who: kutasinaelena
    note: POST /api/notifications/journal with objectIds=[orderId], objectType=CustomerOrder returned the order's NewOrderStatusEmailNotification and OrderCreateEmailNotification rows with rendered bodies; objectType with the full CLR name returned 0.
    build: ad2e031743ee94a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 594519586290ed24
    at: 2026-10-22T13:49:21.792Z
    by: session:9b586f75
    bodyHash: fd5fca579e50
    who: Lenajava1
    note: POST /api/notifications/journal keyword=<recipient email> returned MfaSignInEmailNotification rows with the rendered body and the 6-digit code; status Pending at first, Error later (SendGrid credits).
  - method: observation
    deployment: vcst_qa
    build: d70745c02c9df3fe
    at: 2026-10-23T11:39:34.493Z
    by: session:7ed484a4
    bodyHash: fd5fca579e50
    who: kutasinaelena
    note: "2026-10-23: MfaSignInEmailNotification row for the recipient was readable 0.5-0.6 s after the password grant (createdDate 0.2 s after it), status Pending, body carrying the 6-digit code."
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4
    at: 2026-11-18T13:49:05.195Z
    by: session:6da8340c
    bodyHash: fd5fca579e50
    who: Lenajava1
    note: POST /api/notifications/journal with keyword=<buyer email>, sort createdDate:desc returned OtpSignInEmailNotification rows with the rendered body and the 6-digit code in <strong>, readable within about 1-3 s of POST /api/otp/request (3 cases, 6 codes).
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4
    at: 2026-11-18T14:01:36.899Z
    by: session:6da8340c
    bodyHash: fd5fca579e50
    who: Lenajava1
    note: POST /api/notifications/journal keyword=<buyer email>, sort createdDate:desc returned OtpSignInEmailNotification rows (status Sent) with the 6-digit code in the body, readable about 1-3 s after POST /api/otp/request.
---
POST /api/notifications/journal with {notificationType, keyword: <recipient email>, sort: 'createdDate:desc'} returns EmailNotificationMessage rows that already include from, to, cc, subject, body and tenantIdentity, so GET /api/notifications/journal/{id} is not needed for the body. keyword matched the to field (all hits had that recipient when combined with notificationType); keyword alone matched far more rows, so filter to client-side. On a deployment without SMTP the row is stored with status Error but the rendered body is present. to can hold several recipients separated by '; '.
