---
id: KB-7B7DF7F1
subject: rest-api-platform-notification
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/notification, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Notifications
    version: 3.1001.8
anchors:
  - coordinate: POST /api/platform/notification/template/sendnotification
    operationId: Notifications_SendNotificationByRequest
    hash: 39a4ee903741
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/notification

1 operation under `/api/platform/notification`, served by module `VirtoCommerce.Notifications`, published under the tag "Notifications".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `POST /api/platform/notification/template/sendnotification`<br>`Notifications_SendNotificationByRequest` | Sending notification | body application/json (optional) | `NotificationSendResult` |

Module `VirtoCommerce.Notifications` — Notifications: Provides a comprehensive infrastructure for managing and delivering notifications within the Virto Commerce platform.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-notification.json`.
