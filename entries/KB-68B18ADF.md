---
id: KB-68B18ADF
subject: Page Builder shell Archive confirmation Cancel sends no request, shows no toast and leaves the page unchanged
plane: experiential
question: What happens when you cancel the Archive confirmation in the Page Builder shell page-details blade?
status: active
appliesTo:
  - axis: module
    value: page-builder
  - axis: role
    value: administrator
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell
  - coordinate: POST /api/page-builder-pages/grouped/archive
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:38:48.223Z
    by: session:f1cfecd7
    who: Lenajava1
---
On PageBuilderModule 3.1034.0-pr-172-5aad as administrator, in /apps/page-builder-shell page-details blade of a Draft page: toolbar Archive opens a 'Confirmation' dialog ('Are you sure you want to archive this page?', Confirm / Cancel). Cancel closes the dialog; no POST /api/page-builder-pages/grouped/archive is sent (network log unchanged over 6 s), no toast appears, the blade stays open with the 'Draft' chip and the Draft counter is unchanged. Confirm sends POST /api/page-builder-pages/grouped/archive?ids={groupId} -> 204, closes the blade and shows 'Page archived successfully' once.
