---
id: KB-B2E4D4A5
subject: Page Builder shell Publish, Unpublish and Archive complete without any success toast, while Save, Save content and Load content do show one
plane: experiential
question: Does the Page Builder shell show a success notification after Publish, Unpublish or Archive of a page?
status: active
appliesTo:
  - axis: module
    value: page-builder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell
  - coordinate: POST /api/page-builder-pages/grouped/publishing/{groupId}
  - coordinate: POST /api/page-builder-pages/grouped/archive
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T08:45:04.278Z
    by: session:b1874592
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T14:06:31.514Z
    by: session:f1cfecd7
    who: Lenajava1
    note: Same on PageBuilderModule 3.1034.0-pr-167-0ed1. In the shell details blade, Publish (publishing?publish=true → 200) and Unpublish (?publish=false → 200) rendered no notification in snapshots taken from the click to +15 s, while Save showed 'Page saved successfully'. The only signals were the status chip (Draft ↔ Published) and the toolbar button swapping Publish ↔ Unpublish.
---
On PageBuilderModule 3.1033.0-pr-170-236e (vc-shell framework 2.6.0), as administrator in the page-builder-shell page details blade: Publish (POST /api/page-builder-pages/grouped/publishing/{groupId}?publish=true -> 200), Unpublish (?publish=false -> 200) and Archive (Confirmation -> Confirm) all succeed (status changes in UI and REST) but no notification is rendered (wait 5s, screenshots right after click). Save shows 'Page saved successfully', Save content shows 'Content saved to file successfully', Load content shows 'Content loaded successfully'. Source: the publish/unpublish/delete toolbar handlers in PageDetails.vue have no notification.success call; they were dropped in the @vc-shell 2.0.3 upgrade commit (VCST-5105), although the PUBLISH_SUCCESS/UNPUBLISH_SUCCESS locale keys still exist. Clone and saving an imported page call notification.success but the toast is also not visible after the blade swaps to the new page.
