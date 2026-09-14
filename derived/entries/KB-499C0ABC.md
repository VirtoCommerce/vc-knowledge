---
id: KB-499C0ABC
subject: rest-api-changes
plane: derived-first
question: Which endpoints does this deployment serve under /api/changes, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /api/changes/lastmodifieddate
    operationId: ChangeLog_GetLastModifiedDate
    hash: 07028f872360
  - coordinate: POST /api/changes/changed-entities
    operationId: ChangeLog_GetChangedEntities
    hash: 8a96b81ff247
  - coordinate: POST /api/changes/changed-entities/reset
    operationId: ChangeLog_ResetChangedEntities
    hash: 96ad2da10c60
  - coordinate: POST /api/changes/force
    operationId: ChangeLog_ForceChanges
    hash: 8c4bfe833f6b
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/changes

4 operations under `/api/changes`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/changes/lastmodifieddate`<br>`ChangeLog_GetLastModifiedDate` | Get last modified date for given scope Used for signal of what something changed and for cache invalidation in external platform clients | — | `LastModifiedResponse` |
| `POST /api/changes/changed-entities`<br>`ChangeLog_GetChangedEntities` | — | body application/json (optional) | `ChangedEntitiesResponse` |
| `POST /api/changes/changed-entities/reset`<br>`ChangeLog_ResetChangedEntities` | — | body `string[]` (optional) | 204 |
| `POST /api/changes/force`<br>`ChangeLog_ForceChanges` | Force set changes last modified date | — | 204 |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-changes.json`.
