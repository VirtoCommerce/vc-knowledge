---
id: KB-877AE3BD
subject: rest-api-files
plane: derived-first
question: Which endpoints does this deployment serve under /api/files, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.FileExperienceApi
    version: 3.1000.0
anchors:
  - coordinate: GET /api/files/{id}
    operationId: FileUpload_DownloadFile
    hash: 5138ba71a045
  - coordinate: POST /api/files/{scope}
    operationId: FileUpload_UploadFiles
    hash: dc447c27fdd7
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/files

2 operations under `/api/files`, served by module `VirtoCommerce.FileExperienceApi`, published under the tag "File Experience API".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/files/{id}`<br>`FileUpload_DownloadFile` | — | `id` (path) | 200 |
| `POST /api/files/{scope}`<br>`FileUpload_UploadFiles` | — | `scope` (path), body `object` — required: `file` | `FileUploadResult[]` |

Module `VirtoCommerce.FileExperienceApi` — File Experience API: Manages all file-related operations, from file upload to file content download for different client applications.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-files.json`.
