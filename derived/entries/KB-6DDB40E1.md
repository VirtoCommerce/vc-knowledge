---
id: KB-6DDB40E1
subject: rest-api-gdpr
plane: derived-first
question: Which endpoints does this deployment serve under /api/gdpr, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.GDPR
    version: 3.1000.0
anchors:
  - coordinate: GET /api/gdpr/contacts/anonymize/{id}
    operationId: Gdpr_AnonymizeContact
    hash: abf7a0185b84
  - coordinate: GET /api/gdpr/contacts/download/{id}
    operationId: Gdpr_DownloadContactInfo
    hash: 4be8218c6801
  - coordinate: POST /api/gdpr/contacts/search
    operationId: Gdpr_GetContactList
    hash: 4f3a2ee7e97d
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/gdpr

3 operations under `/api/gdpr`, served by module `VirtoCommerce.GDPR`, published under the tag "GDPR".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/gdpr/contacts/anonymize/{id}`<br>`Gdpr_AnonymizeContact` | — | `id` (path) | 204 |
| `GET /api/gdpr/contacts/download/{id}`<br>`Gdpr_DownloadContactInfo` | — | `id` (path) | `Customer` |
| `POST /api/gdpr/contacts/search`<br>`Gdpr_GetContactList` | — | body application/json (optional) | `Contact[]` |

Module `VirtoCommerce.GDPR` — GDPR: Apply General Data Protection Regulation policies with review or remove their personal details, by anonymizing them, from your online store.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-gdpr.json`.
