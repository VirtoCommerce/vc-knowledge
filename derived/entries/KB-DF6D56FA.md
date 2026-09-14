---
id: KB-DF6D56FA
subject: rest-api-assets
plane: derived-first
question: Which endpoints does this deployment serve under /api/assets, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Assets
    version: 3.1001.2
anchors:
  - coordinate: DELETE /api/assets
    operationId: Assets_DeleteBlobsAsync
    hash: cb289e54c596
  - coordinate: GET /api/assets
    operationId: Assets_SearchAssetItemsAsync
    hash: a4cca9269505
  - coordinate: POST /api/assets
    operationId: Assets_UploadAssetAsync
    hash: 5148add3ecd2
  - coordinate: POST /api/assets/folder
    operationId: Assets_CreateBlobFolderAsync
    hash: a8d3a771386d
  - coordinate: POST /api/assets/localstorage
    operationId: Assets_UploadAssetToLocalFileSystemAsync
    hash: 16b7e8407afb
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/assets

5 operations under `/api/assets`, served by module `VirtoCommerce.Assets`, published under the tag "Assets Management".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/assets`<br>`Assets_DeleteBlobsAsync` | Delete blobs by urls | — | 204 |
| `GET /api/assets`<br>`Assets_SearchAssetItemsAsync` | SearchAsync asset folders and blobs | — | `BlobEntrySearchResult` |
| `POST /api/assets`<br>`Assets_UploadAssetAsync` | Upload assets to the folder | body `object` — fields: `file` (optional) | `BlobInfo[]` |
| `POST /api/assets/folder`<br>`Assets_CreateBlobFolderAsync` | Create new blob folder | body application/json (optional) | 204 |
| `POST /api/assets/localstorage`<br>`Assets_UploadAssetToLocalFileSystemAsync` | This method used to upload files on local disk storage in special uploads folder | body `object` — required: `file` | `BlobInfo[]` |

Module `VirtoCommerce.Assets` — Assets Management: Common abstractions for asset search, retrieval, and manipulation, making it easy for developers to work with assets regardless of their underlying storage location.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-assets.json`.
