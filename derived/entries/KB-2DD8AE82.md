---
id: KB-2DD8AE82
subject: rest-api-contacts
plane: derived-first
question: Which endpoints does this deployment serve under /api/contacts, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Customer
    version: 3.1000.7
anchors:
  - coordinate: DELETE /api/contacts
    operationId: CustomerModule_DeleteContacts
    hash: e93d89f55f93
  - coordinate: GET /api/contacts
    operationId: CustomerModule_GetContactsByIds
    hash: a9b8f3df4b8c
  - coordinate: GET /api/contacts/{id}
    operationId: CustomerModule_GetContactById
    hash: 1ea486977b36
  - coordinate: PATCH /api/contacts/{id}
    operationId: CustomerModule_PatchContact
    hash: 424379ead5ae
  - coordinate: POST /api/contacts
    operationId: CustomerModule_CreateContact
    hash: 314c063caf88
  - coordinate: POST /api/contacts/bulk
    operationId: CustomerModule_BulkCreateContacts
    hash: a438f1cf90a1
  - coordinate: POST /api/contacts/search
    operationId: CustomerModule_SearchContacts
    hash: 7d7d83ead63a
  - coordinate: PUT /api/contacts
    operationId: CustomerModule_UpdateContact
    hash: 0bbfdf50cd0e
  - coordinate: PUT /api/contacts/bulk
    operationId: CustomerModule_BulkUpdateContacts
    hash: 3291906e1790
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/contacts

9 operations under `/api/contacts`, served by module `VirtoCommerce.Customer`, published under the tag "Companies and Contacts".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/contacts`<br>`CustomerModule_DeleteContacts` | Delete contacts | — | 204 |
| `GET /api/contacts`<br>`CustomerModule_GetContactsByIds` | Get plenty contacts | — | `Contact[]` |
| `GET /api/contacts/{id}`<br>`CustomerModule_GetContactById` | Get contact | `id` (path) | `Contact` |
| `PATCH /api/contacts/{id}`<br>`CustomerModule_PatchContact` | Partial update for Contact | `id` (path), body `Operation[]` (optional) | 204 |
| `POST /api/contacts`<br>`CustomerModule_CreateContact` | Create contact | body application/json (optional) | `Contact` |
| `POST /api/contacts/bulk`<br>`CustomerModule_BulkCreateContacts` | Bulk create contacts | body `Contact[]` (optional) | `Contact[]` |
| `POST /api/contacts/search`<br>`CustomerModule_SearchContacts` | Search contacts | body application/json (optional) | `ContactSearchResult` |
| `PUT /api/contacts`<br>`CustomerModule_UpdateContact` | Update contact | body application/json (optional) | 204 |
| `PUT /api/contacts/bulk`<br>`CustomerModule_BulkUpdateContacts` | Bulk update contact | body `Contact[]` (optional) | 204 |

Module `VirtoCommerce.Customer` — Companies and Contacts: Managing customers contacts and organizations

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-contacts.json`.
