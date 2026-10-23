---
id: KB-2CA7D82C
subject: create-group-page answers 403 to a non-admin back-office user holding builder:access/read/update but not builder:create, before any body validation
plane: experiential
question: What does POST /api/page-builder-pages/create-group-page return for a user without builder:create?
questions:
  - text: Is builder:create checked before the permalink validation on create-group-page?
  - text: Which permission gates PageBuilder page creation via REST?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-pages/create-group-page
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2; role=builder:access+builder:read+builder:update
    at: 2026-10-23T10:25:08.830Z
    by: session:562b04df
    who: Aleksandra-Mitricheva
    note: "distinct: KB-3642CE3E is about the request body an authorized caller must send; this is about the permission gate for a caller without builder:create, and that it fires before body validation."
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2; role=builder:access+builder:read+builder:update
    at: 2026-10-23T10:47:52.864Z
    by: session:562b04df
    bodyHash: 7832662ab821
    who: Aleksandra-Mitricheva
    note: "Also with INVALID permalinks (inner space, %, %20): create-group-page and POST grouped both return 403 empty body for the no-create user and nothing is stored, so the permission gate precedes permalink validation on both endpoints."
---
A Manager account (isAdministrator=false) whose only role grants builder:access, builder:read and builder:update obtained a token at /connect/token and sent POST /api/page-builder-pages/create-group-page with a complete, valid body (storeId, cultureName, name, a valid permalink, content '{"settings":{},"content":[]}'): HTTP 403. The same body sent by an administrator returned 200. An earlier probe from that user without content also got 403 (not the 400 'StoreId, Name and Content are required.'), so the builder:create authorization gate runs before body and permalink validation.
