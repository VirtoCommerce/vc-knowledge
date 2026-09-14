---
id: KB-D5D1DDA1
subject: rest-api-platform-pushnotifications
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/pushnotifications, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: POST /api/platform/pushnotifications
    operationId: PushNotification_SearchPushNotification
    hash: 152dd932996b
  - coordinate: POST /api/platform/pushnotifications/markAllAsRead
    operationId: PushNotification_MarkAllAsRead
    hash: a9bf68128aaf
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/pushnotifications

2 operations under `/api/platform/pushnotifications`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `POST /api/platform/pushnotifications`<br>`PushNotification_SearchPushNotification` | SearchAsync push notifications | body application/json (optional) | `PushNotificationSearchResult` |
| `POST /api/platform/pushnotifications/markAllAsRead`<br>`PushNotification_MarkAllAsRead` | Mark all notifications as read | — | `PushNotificationSearchResult` |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-pushnotifications.json`.
