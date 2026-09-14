---
id: KB-F94BFF5C
subject: rest-api-platform-localizable-settings
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/localizable-settings, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: DELETE /api/platform/localizable-settings/{name}/dictionary-items
    operationId: LocalizableSettings_DeleteDictionaryItems
    hash: 36972610908c
  - coordinate: GET /api/platform/localizable-settings
    operationId: LocalizableSettings_GetSettingsAndLanguages
    hash: 56ed78a0a85e
  - coordinate: GET /api/platform/localizable-settings/{name}/dictionary-items/{language}/values
    operationId: LocalizableSettings_GetDictionaryValues
    hash: 5c29a48434ee
  - coordinate: POST /api/platform/localizable-settings/{name}/dictionary-items
    operationId: LocalizableSettings_SaveDictionaryItems
    hash: 472499ac44e3
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/localizable-settings

4 operations under `/api/platform/localizable-settings`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/platform/localizable-settings/{name}/dictionary-items`<br>`LocalizableSettings_DeleteDictionaryItems` | — | `name` (path) | 204 |
| `GET /api/platform/localizable-settings`<br>`LocalizableSettings_GetSettingsAndLanguages` | — | — | `LocalizableSettingsAndLanguages` |
| `GET /api/platform/localizable-settings/{name}/dictionary-items/{language}/values`<br>`LocalizableSettings_GetDictionaryValues` | — | `language` (path), `name` (path) | `KeyValue[]` |
| `POST /api/platform/localizable-settings/{name}/dictionary-items`<br>`LocalizableSettings_SaveDictionaryItems` | — | `name` (path), body `DictionaryItem[]` (optional) | 204 |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-localizable-settings.json`.
