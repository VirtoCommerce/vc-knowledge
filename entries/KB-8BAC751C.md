---
id: KB-8BAC751C
subject: ProductSubscription 3.1000.0-pr-1-595d registers 7 email notifications with file-system templates, but its package ships no NotificationTemplates folder, so every module email fails with empty Subject and Body
plane: experiential
question: Why do Product Subscription emails fail with 'Subject' must not be empty and why does GET /api/notifications/SubscriptionCreatedEmailNotification return no templates?
questions:
  - text: Does the Product Subscription module ship default email templates?
  - text: Why are SubscriptionCreatedEmailNotification templates empty at global and store level?
  - text: Why do renewal reminder emails fail in the notification journal?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.productsubscription
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/notifications/SubscriptionCreatedEmailNotification
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1099.0; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; module:VirtoCommerce.Notifications=3.1019.0; role=administrator
    defect: suspected
    at: 2026-11-16T18:24:20.240Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 9dfbb57eacb0bf2c
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-79cf; platform=3.1099.0
    at: 2026-11-17T10:47:26.455Z
    by: session:d9b8e380
    bodyHash: 629cf0c38052
    who: kutasinaelena
    contradicts: true
    contested: module:VirtoCommerce.ProductSubscription 3.1000.0-pr-1-79cf vs 3.1000.0-pr-1-595d
    note: On 3.1000.0-pr-1-79cf the predefined templates are present (GET /api/notifications/<type>?tenantType=Store returns one template) and SubscriptionCreated, SubscriptionCancelled, RenewalReminder, RenewalReviewRequired and RenewalPaymentActionRequired journal rows render with recipient, subject and body (they fail only on SMTP). Only RenewalPaymentFailedEmailNotification still has To/Subject/Body empty (KB-94EB036A).
---
As admin via REST: the notification journal holds only Error rows for SubscriptionCreated, SubscriptionCancelled, RenewalReminder, RenewalReviewRequired and RenewalPaymentActionRequired email notifications (27 of 27 in the latest 50 rows), lastSendError "'Subject' must not be empty. 'Body' must not be empty.", subject and body null. GET /api/notifications/SubscriptionCreatedEmailNotification returns templates [] both globally and with tenantId=B2B-store&tenantType=Store. The module registers each notification WithTemplatesFromPath({moduleRoot}/NotificationTemplates) and the source repo contains 7 subject/body template pairs, but the deployed module zip (35 entries) contains no NotificationTemplates folder at root or under bin: the Web csproj uses a Content Update item for NotificationTemplates, which matches no item, unlike order/customer modules that copy a NotificationTemplates item group to the module root after Publish. Until fixed, an admin must enter templates by hand for each notification.
