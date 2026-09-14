---
id: KB-5FA876BD
subject: rest-api-push-message
plane: derived-first
question: Which endpoints does this deployment serve under /api/push-message, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.PushMessages
    version: 3.1000.0
anchors:
  - coordinate: DELETE /api/push-message
    operationId: PushMessage_Delete
    hash: aa7ee578fab3
  - coordinate: GET /api/push-message/{id}
    operationId: PushMessage_Get
    hash: d31a225ec943
  - coordinate: POST /api/push-message
    operationId: PushMessage_Create
    hash: f8e1ae1620dc
  - coordinate: POST /api/push-message/search
    operationId: PushMessage_Search
    hash: 1d96555ae345
  - coordinate: POST /api/push-message/search-recipients
    operationId: PushMessage_SearchRecipients
    hash: 99e84ebe9054
  - coordinate: PUT /api/push-message
    operationId: PushMessage_Update
    hash: 81996b5c302f
  - coordinate: PUT /api/push-message/{id}/tracking/{value}
    operationId: PushMessage_ChangeTracking
    hash: 1af84ca21349
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/push-message

7 operations under `/api/push-message`, served by module `VirtoCommerce.PushMessages`, published under the tag "Push Messages".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/push-message`<br>`PushMessage_Delete` | — | — | 204 |
| `GET /api/push-message/{id}`<br>`PushMessage_Get` | — | `id` (path) | `PushMessage` |
| `POST /api/push-message`<br>`PushMessage_Create` | — | body application/json (optional) | `PushMessage` |
| `POST /api/push-message/search`<br>`PushMessage_Search` | — | body application/json (optional) | `PushMessageSearchResult` |
| `POST /api/push-message/search-recipients`<br>`PushMessage_SearchRecipients` | — | body application/json (optional) | `PushMessageRecipientSearchResult` |
| `PUT /api/push-message`<br>`PushMessage_Update` | — | body application/json (optional) | `PushMessage` |
| `PUT /api/push-message/{id}/tracking/{value}`<br>`PushMessage_ChangeTracking` | — | `id` (path), `value` (path) | `PushMessage` |

Module `VirtoCommerce.PushMessages` — Push Messages: Enables back-end admins to send custom notifications to selected organizations within the Virto Commerce platform.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-push-message.json`.
