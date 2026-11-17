---
id: KB-ACA4D4A3
subject: DELETE /api/page-builder-pages/grouped/{groupId} on a published (or published-then-unpublished) group returns 200 {pageDeleted:false} and the group stays; POST grouped/archive?ids= then DELETE removes it
plane: experiential
question: How do you delete a Page Builder database page group that has been published?
status: active
appliesTo:
  - axis: module
    value: pagebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: DELETE /api/page-builder-pages/grouped/{id}
  - coordinate: POST /api/page-builder-pages/grouped/archive
evidence:
  - method: observation
    deployment: vcptcore_stable
    build: 3d715345b37f50ec
    conditions: platform=3.1039.19; module:VirtoCommerce.PageBuilderModule=3.1012.4
    at: 2026-11-17T12:01:07.828Z
    by: session:a434dd08
    who: kutasinaelena
    note: "distinct: KB-58C11A20 covers a never-published Draft, which DELETE removes; this is a group that was ever published, which DELETE leaves in place until it is archived."
---
On a group that had been published (with Archived versions), DELETE grouped/{id} answered 200 {"pageDeleted":false,"indexDeleted":true} and GET grouped/{id} still returned 200; unpublishing first (publishing?publish=false) changed nothing. POST /api/page-builder-pages/grouped/archive?ids=<id>&ids=<id> (204) followed by DELETE grouped/{id} made GET return 404 and the keyword search return 0.
