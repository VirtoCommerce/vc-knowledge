---
id: KB-9EFC5A55
subject: rest-api-customer-preferences
plane: derived-first
question: Which endpoints does this deployment serve under /api/customer-preferences, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Customer
    version: 3.1000.7
anchors:
  - coordinate: DELETE /api/customer-preferences
    operationId: CustomerPreference_Delete
    hash: 1edb2056f1ac
  - coordinate: GET /api/customer-preferences/{id}
    operationId: CustomerPreference_Get
    hash: 451b96150b24
  - coordinate: POST /api/customer-preferences
    operationId: CustomerPreference_Create
    hash: d6dc467c01cb
  - coordinate: POST /api/customer-preferences/search
    operationId: CustomerPreference_Search
    hash: 3a0b83a56425
  - coordinate: PUT /api/customer-preferences
    operationId: CustomerPreference_Update
    hash: 2f45c3004f27
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/customer-preferences

5 operations under `/api/customer-preferences`, served by module `VirtoCommerce.Customer`, published under the tag "Companies and Contacts".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/customer-preferences`<br>`CustomerPreference_Delete` | — | — | 204 |
| `GET /api/customer-preferences/{id}`<br>`CustomerPreference_Get` | — | `id` (path) | `CustomerPreference` |
| `POST /api/customer-preferences`<br>`CustomerPreference_Create` | — | body application/json (optional) | `CustomerPreference` |
| `POST /api/customer-preferences/search`<br>`CustomerPreference_Search` | — | body application/json (optional) | `CustomerPreferenceSearchResult` |
| `PUT /api/customer-preferences`<br>`CustomerPreference_Update` | — | body application/json (optional) | `CustomerPreference` |

Module `VirtoCommerce.Customer` — Companies and Contacts: Managing customers contacts and organizations

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-customer-preferences.json`.
