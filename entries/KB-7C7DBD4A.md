---
id: KB-7C7DBD4A
subject: POST /api/site-builder-pages/create-group-page rejects a content string whose root is a JSON array with 400; an object root with settings and a content array is stored as sent
plane: experiential
question: Does POST /api/site-builder-pages/create-group-page accept a legacy top-level array as content?
questions:
  - text: What does create-group-page return when content is a JSON array?
  - text: Does the Site Builder API convert legacy array page content on create?
  - text: Which content shape does create-group-page require?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder-pages/create-group-page
  - coordinate: POST /api/site-builder-pages/grouped
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    at: 2026-11-20T07:27:54.338Z
    by: session:07a3812a
    who: Lenajava1
    note: "distinct: This is the REST contract of create-group-page called directly (array root -> 400, object root stored as sent), not the shell's Load content behaviour that KB-2C71829C describes."
---
Admin bearer token, B2B-store: POST /api/site-builder-pages/create-group-page with content = JSON string of an array of two text sections -> 400 "Content root must be a JSON object with `settings` and `content` keys." The same call with content {settings:{}, content:[s1,s2]} (with or without version:1) -> 200, and GET /grouped/{id}/content?draft=true returns both sections unchanged. The API does no legacy-array conversion; any conversion is the shell's job before the call. POST /api/site-builder-pages/grouped without content creates the draft with content [] and visibility false.
