---
id: KB-BF2C63B3
subject: rest-api-platform-developer-tools
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/developer-tools, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /api/platform/developer-tools
    operationId: DeveloperTools_GetDeveloperTools
    hash: 6805365af7c8
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/developer-tools

1 operation under `/api/platform/developer-tools`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/platform/developer-tools`<br>`DeveloperTools_GetDeveloperTools` | — | — | `DeveloperToolDescriptor[]` |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-developer-tools.json`.
