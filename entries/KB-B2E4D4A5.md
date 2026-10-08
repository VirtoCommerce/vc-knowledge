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
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:38:35.327Z
    by: session:f1cfecd7
    who: Lenajava1
    contradicts: true
    note: "On PageBuilderModule 3.1034.0-pr-172-5aad (PR #172, admin, Edge), the shell page-details blade shows exactly one success toast for each action: Publish 'Page published successfully' (6/6), Unpublish 'Page unpublished successfully' (3/3), Archive 'Page archived successfully' (10/10, still visible after the blade closes). The toast is a listitem in the top-level notification list with role=status and a 'Dismiss notification' button, and it disappears about 3 s after the click. Two parts of the entry also did not hold on this build. Clone does show 'Page cloned successfully' once: a polling wait caught it right after the blade swapped to the copy, while sampled snapshots taken at about +1 s and +5 s missed it. Saving an imported (Load content) page shows 'Page saved successfully' once. The entry's 'no toast' for publish/unpublish/archive was true for the pre-fix builds 3.1033.0-pr-170 and 3.1034.0-pr-167."
---
On PageBuilderModule 3.1033.0-pr-170-236e (vc-shell framework 2.6.0), as administrator in the page-builder-shell page details blade: Publish (POST /api/page-builder-pages/grouped/publishing/{groupId}?publish=true -> 200), Unpublish (?publish=false -> 200) and Archive (Confirmation -> Confirm) all succeed (status changes in UI and REST) but no notification is rendered (wait 5s, screenshots right after click). Save shows 'Page saved successfully', Save content shows 'Content saved to file successfully', Load content shows 'Content loaded successfully'. Source: the publish/unpublish/delete toolbar handlers in PageDetails.vue have no notification.success call; they were dropped in the @vc-shell 2.0.3 upgrade commit (VCST-5105), although the PUBLISH_SUCCESS/UNPUBLISH_SUCCESS locale keys still exist. Clone and saving an imported page call notification.success but the toast is also not visible after the blade swaps to the new page.
