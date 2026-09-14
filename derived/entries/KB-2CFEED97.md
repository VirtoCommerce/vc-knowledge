---
id: KB-2CFEED97
subject: rest-api-stores
plane: derived-first
question: Which endpoints does this deployment serve under /api/stores, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Store
    version: 3.1000.1
anchors:
  - coordinate: DELETE /api/stores
    operationId: StoreModule_DeleteStore
    hash: 9c51744d4363
  - coordinate: GET /api/stores/{id}
    operationId: StoreModule_GetStoreById
    hash: 0cdfeeeae944
  - coordinate: GET /api/stores/{id}/public-settings
    operationId: StoreModule_GetStorePublicSettingsById
    hash: c377e707c226
  - coordinate: GET /api/stores/{storeId}/accounts/{id}/loginonbehalf
    operationId: StoreModule_GetLoginOnBehalfInfo
    hash: 83e334fe8f57
  - coordinate: GET /api/stores/allowed/{userId}
    operationId: StoreModule_GetUserAllowedStores
    hash: 9f69013a0228
  - coordinate: GET /api/stores/outer/{outerId}
    operationId: StoreModule_GetStoreByOuterId
    hash: 12790c986d39
  - coordinate: PATCH /api/stores/{id}
    operationId: StoreModule_PatchStore
    hash: e978be3fd267
  - coordinate: POST /api/stores
    operationId: StoreModule_CreateStore
    hash: 1e3eb1fb4308
  - coordinate: POST /api/stores/search
    operationId: StoreModule_SearchStores
    hash: bdbfb756e3d8
  - coordinate: POST /api/stores/send/dynamicnotification
    operationId: StoreModule_SendDynamicNotificationToStoreEmail
    hash: b15f9787fcf6
  - coordinate: PUT /api/stores
    operationId: StoreModule_UpdateStore
    hash: 840ee314e217
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/stores

11 operations under `/api/stores`, served by module `VirtoCommerce.Store`, published under the tag "Store".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/stores`<br>`StoreModule_DeleteStore` | Delete stores | — | 204 |
| `GET /api/stores/{id}`<br>`StoreModule_GetStoreById` | Get store by id | `id` (path) | `Store` |
| `GET /api/stores/{id}/public-settings`<br>`StoreModule_GetStorePublicSettingsById` | — | `id` (path) | `ModulePublicStoreSettings[]` |
| `GET /api/stores/{storeId}/accounts/{id}/loginonbehalf`<br>`StoreModule_GetLoginOnBehalfInfo` | Check if given contact has login on behalf permission | `id` (path), `storeId` (path) | `LoginOnBehalfInfo` |
| `GET /api/stores/allowed/{userId}`<br>`StoreModule_GetUserAllowedStores` | Returns list of stores which user can sign in | `userId` (path) | `Store[]` |
| `GET /api/stores/outer/{outerId}`<br>`StoreModule_GetStoreByOuterId` | Gets store by outer id. | `outerId` (path) | `Store` |
| `PATCH /api/stores/{id}`<br>`StoreModule_PatchStore` | Partial update for the specified store by id | `id` (path), body `Operation[]` (optional) | 204 |
| `POST /api/stores`<br>`StoreModule_CreateStore` | Create store | body application/json (optional) | `Store` |
| `POST /api/stores/search`<br>`StoreModule_SearchStores` | Search stores | body application/json (optional) | `StoreSearchResult` |
| `POST /api/stores/send/dynamicnotification`<br>`StoreModule_SendDynamicNotificationToStoreEmail` | Send dynamic notification (contains custom list of properties) to store or administrator email | body application/json (optional) | 204 |
| `PUT /api/stores`<br>`StoreModule_UpdateStore` | Update store | body application/json (optional) | 204 |

Module `VirtoCommerce.Store` — Store: Multi store management with individual store settings

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-stores.json`.
