---
id: KB-1E7E7F2F
subject: rest-api-addresses
plane: derived-first
question: Which endpoints does this deployment serve under /api/addresses, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Customer
    version: 3.1000.7
anchors:
  - coordinate: PUT /api/addresses
    operationId: CustomerModule_UpdateAddesses
    hash: 7a1f8fb90bdd
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/addresses

1 operation under `/api/addresses`, served by module `VirtoCommerce.Customer`, published under the tag "Companies and Contacts".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `PUT /api/addresses`<br>`CustomerModule_UpdateAddesses` | — | body `CustomerAddress[]` (optional) | 204 |

Module `VirtoCommerce.Customer` — Companies and Contacts: Managing customers contacts and organizations

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-addresses.json`.
