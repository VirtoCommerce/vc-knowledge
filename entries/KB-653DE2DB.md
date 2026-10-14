---
id: KB-653DE2DB
subject: "Page Builder asset endpoints split permissions: /search needs platform:asset:read, /references needs Page Builder read"
plane: experiential
question: Which permissions gate POST /api/page-builder-assets/search and /references?
status: active
appliesTo:
  - axis: aspect
    value: rbac
  - axis: module
    value: page-builder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-assets/search
  - coordinate: POST /api/page-builder-assets/references
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-14T13:46:51.741Z
    by: session:8b33e471
    who: kutasinaelena
---
On PageBuilderModule 3.1035.0-pr-171-61df: a non-admin Manager with only builder:access+builder:read got 403 on /search (and on GET /api/assets) but 200 on /references (including page names). A non-admin Manager with only platform:asset:access+platform:asset:read got 200 on /search and 403 on /references (per-asset, includePages false, and folderUrl forms).
