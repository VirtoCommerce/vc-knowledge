---
id: KB-95CDAF99
subject: rest-api-platform-diagnostics
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/diagnostics, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /api/platform/diagnostics/errors
    operationId: Diagnostics_GetModulesErrors
    hash: 3a53bfa94380
  - coordinate: GET /api/platform/diagnostics/systeminfo
    operationId: Diagnostics_GetSystemInfo
    hash: e72ac0f01223
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/diagnostics

2 operations under `/api/platform/diagnostics`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/platform/diagnostics/errors`<br>`Diagnostics_GetModulesErrors` | Get installed modules with errors | — | `ModuleDescriptor[]` |
| `GET /api/platform/diagnostics/systeminfo`<br>`Diagnostics_GetSystemInfo` | — | — | `SystemInfo` |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-diagnostics.json`.
