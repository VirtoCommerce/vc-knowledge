---
id: KB-35AA6EDC
subject: rest-api-platform-apps
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/apps, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /api/platform/apps
    operationId: Apps_GetApps
    hash: 2b8ba0bd6b53
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/apps

1 operation under `/api/platform/apps`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/platform/apps`<br>`Apps_GetApps` | Gets the list of available apps, filtered by user permissions. | — | `AppDescriptor[]` |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-apps.json`.
