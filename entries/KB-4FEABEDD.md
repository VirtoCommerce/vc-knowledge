---
id: KB-4FEABEDD
subject: Site Builder builder:read and builder:shared-components:read carry StoreSelectedScope; a role scoped to one store gets 403 on every Site Builder read and write of another store, while writes inside the store follow the global builder:theme/templates/publish permissions
plane: experiential
question: How do Site Builder permissions scope a back-office editor to one store, and which calls return 403?
questions:
  - text: Can a Site Builder role be limited to a single store?
  - text: What does a store-scoped Site Builder editor get on another store's pages, theme and history?
  - text: Does saving a theme file need builder:theme in Site Builder?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/designer/save
  - coordinate: POST /api/site-builder/save
  - coordinate: POST /api/site-builder-pages/search
  - coordinate: GET /api/platform/security/permissions
  - coordinate: PUT /api/platform/security/roles
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0
    at: 2026-11-19T13:55:42.279Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    at: 2026-11-19T14:20:51.396Z
    by: session:51ac2fe3
    bodyHash: 3443f1c3cd7c
    who: Lenajava1
    note: "Manager user with builder:access, builder:read (scope B2B-store), builder:update, builder:templates, builder:theme and no builder:publish: POST theme/publish?storeId=B2B-store (with and without catalogHash) -> 403 with valid drafts pending, publication unchanged; theme/status B2B-store 200, Electronics 403. A role first created by PUT roles without permission moduleId/role id came back with builder:read assignedScopes [] although storeId was sent; a second PUT with id + moduleId kept the scope."
---
GET /api/platform/security/permissions: builder:read and builder:shared-components:read list availableScopes [StoreSelectedScope]; builder:access/update/templates/theme/publish/create/delete and shared-components create/update/delete have none. A role saved with assignedScopes needs {type:"StoreSelectedScope", storeId, scope, label}; without storeId the scope is silently dropped. Two Manager users without an account store, roles builder:access + builder:read(scope B2B-store) + builder:update (+/- builder:templates): on B2B-store pages/search, grouped/{id}, content, theme/status, designer/theme-file, history/status, history?item=page:, search -> 200; designer/save page draft -> 200; POST /api/site-builder/save draft=true for a config/ theme file -> 403 (no builder:theme); for a templates/ file -> 403 without builder:templates, 400 validation with it; save draft=false for a page and grouped/publishing -> 403 (no builder:publish); shared-components/search -> 403 (no shared-components:read). On Electronics every one of those reads and writes -> 403.
