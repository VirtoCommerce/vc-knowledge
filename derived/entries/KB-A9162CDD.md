---
id: KB-A9162CDD
subject: rest-connect-userinfo
plane: derived-first
question: Which endpoints does this deployment serve under /connect/userinfo, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /connect/userinfo
    operationId: Authorization_Userinfo
    hash: 58815bd14e33
  - coordinate: POST /connect/userinfo
    operationId: Authorization_Userinfo
    hash: c5aebb3a030a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /connect/userinfo

2 operations under `/connect/userinfo`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /connect/userinfo`<br>`Authorization_Userinfo` | — | — | 200 |
| `POST /connect/userinfo`<br>`Authorization_Userinfo` | — | — | 200 |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-connect-userinfo.json`.
