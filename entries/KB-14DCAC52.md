---
id: KB-14DCAC52
subject: GET /api/page-builder-pages/grouped/publish-status/{groupId} returns 500 NullReference for an unknown or deleted groupId instead of 404
plane: experiential
question: What does the publish-status endpoint return for a groupId that does not exist?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-api
anchors:
  - coordinate: GET /api/page-builder-pages/grouped/publish-status/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: ed1246f57f06adbd
    conditions: module:VirtoCommerce.PageBuilderModule=3.1041.0-pr-179-5e6c; module:VirtoCommerce.Pages=3.1013.0-alpha.93-vcst-6442
    defect: suspected
    at: 2026-10-29T13:54:45.411Z
    by: session:9b657bc7
    who: Aleksandra-Mitricheva
---
As administrator, GET /api/page-builder-pages/grouped/publish-status/{random GUID} returned 500 {"message":"Object reference not set to an instance of an object."}. The same happened for a never-published Draft group after POST grouped/archive removed it (GET grouped/{id} then 404). For an existing archived or draft group it returns 200 {published:false,hasChanges:false}. GET grouped/{id} on the same unknown id returns 404, so the 500 is specific to publish-status.
