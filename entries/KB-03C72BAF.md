---
id: KB-03C72BAF
subject: "POST /api/site-builder/save draft=true overwrites an existing theme-file draft with no version check: no ETag is sent or required, while designer/save requires each theme file's ETag (428) and refuses a stale one (412)"
plane: experiential
question: Does the legacy /api/site-builder/save route check the version of a theme draft it overwrites?
questions:
  - text: Can two clients overwrite each other's Site Builder theme draft through /api/site-builder/save?
  - text: Does /api/site-builder/save need an eTag for theme files?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/save
  - coordinate: POST /api/site-builder/designer/save
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    at: 2026-11-19T14:56:42.613Z
    by: session:51ac2fe3
    who: Lenajava1
    note: "distinct: KB-AEDC697F is about content validation (schema catalog) on /save; this is about concurrency: /save overwrites a pending theme draft without any ETag version check, which designer/save enforces with 428/412."
---
B2B-store, admin: with a header-group.json theme draft already pending (written by an earlier legacy save), POST /api/site-builder/save?storeId=B2B-store&theme=default&draft=true with files=[{path:"default/sections/header-group.json",type:"themes",content:...}] and no eTag anywhere -> 200 and designer/theme-file returned the new content, replacing the pending draft. The request model of /save carries no eTag field (SaveFileModel: path, type, content), whereas DesignerOperationService refuses a theme file without ETag ("Read each theme file with designer/theme-file and send its ETag before saving.") and the PR documents 412 on a stale theme file for designer/save only. So a /save client can silently overwrite another editor's theme draft.
