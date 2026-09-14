---
id: KB-2FE07BA9
subject: rest-api-employees
plane: derived-first
question: Which endpoints does this deployment serve under /api/employees, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Customer
    version: 3.1000.7
anchors:
  - coordinate: GET /api/employees
    operationId: CustomerModule_GetEmployeesByIds
    hash: 3f3f00f03ce9
  - coordinate: POST /api/employees
    operationId: CustomerModule_CreateEmployee
    hash: cc45d76b9c9d
  - coordinate: POST /api/employees/bulk
    operationId: CustomerModule_BulkCreateEmployees
    hash: 9d002a53a92e
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/employees

3 operations under `/api/employees`, served by module `VirtoCommerce.Customer`, published under the tag "Companies and Contacts".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/employees`<br>`CustomerModule_GetEmployeesByIds` | Get plenty employees | — | `Employee[]` |
| `POST /api/employees`<br>`CustomerModule_CreateEmployee` | Create employee | body application/json (optional) | `Employee` |
| `POST /api/employees/bulk`<br>`CustomerModule_BulkCreateEmployees` | Create employee | body `Employee[]` (optional) | `Employee[]` |

Module `VirtoCommerce.Customer` — Companies and Contacts: Managing customers contacts and organizations

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-employees.json`.
