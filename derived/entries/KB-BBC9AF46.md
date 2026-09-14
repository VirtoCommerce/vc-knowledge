---
id: KB-BBC9AF46
subject: rest-externalsignin-callback
plane: derived-first
question: Which endpoints does this deployment serve under /externalsignin/callback, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /externalsignin/callback
    operationId: ExternalSignIn_SignInCallback
    hash: 3d1baa862200
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /externalsignin/callback

1 operation under `/externalsignin/callback`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /externalsignin/callback`<br>`ExternalSignIn_SignInCallback` | — | — | 200 |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-externalsignin-callback.json`.
