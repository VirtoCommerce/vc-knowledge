---
id: KB-B2E4D4A5
subject: Page Builder shell Publish, Unpublish and Archive complete without any success toast on the released module (through 3.1034.0), while Save, Clone, Load content and saving an imported page do show one
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
    build: c4000d0bea152759
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T14:06:31.514Z
    by: session:f1cfecd7
    who: Lenajava1
    note: Same on PageBuilderModule 3.1034.0-pr-167-0ed1. In the shell details blade, Publish (publishing?publish=true → 200) and Unpublish (?publish=false → 200) rendered no notification in snapshots taken from the click to +15 s, while Save showed 'Page saved successfully'. The only signals were the status chip (Draft ↔ Published) and the toolbar button swapping Publish ↔ Unpublish.
    build: bb04ff4ad7221cbc
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:38:35.327Z
    by: session:f1cfecd7
    who: Lenajava1
    contradicts: true
    note: "On PageBuilderModule 3.1034.0-pr-172-5aad (PR #172, admin, Edge), the shell page-details blade shows exactly one success toast for each action: Publish 'Page published successfully' (6/6), Unpublish 'Page unpublished successfully' (3/3), Archive 'Page archived successfully' (10/10, still visible after the blade closes). The toast is a listitem in the top-level notification list with role=status and a 'Dismiss notification' button, and it disappears about 3 s after the click. Two parts of the entry also did not hold on this build. Clone does show 'Page cloned successfully' once: a polling wait caught it right after the blade swapped to the copy, while sampled snapshots taken at about +1 s and +5 s missed it. Saving an imported (Load content) page shows 'Page saved successfully' once. The entry's 'no toast' for publish/unpublish/archive was true for the pre-fix builds 3.1033.0-pr-170 and 3.1034.0-pr-167."
    resolved: claim-amended
    resolvedAt: 2026-10-12T19:06:16.341Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: Publish/Unpublish/Archive toasts exist only on the unmerged PR #172 build (released 3.1033.0 shows none, 0/10, tags 3.1033.0/3.1034.0 have no call), and the entry's 'Clone and imported-page toasts not visible' is wrong on the release too (seen 3/3).
    amendedFrom: 3f83185ea926
    build: 2f9a5dce907b5872
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1078.0; module:VirtoCommerce.PageBuilderModule=3.1033.0; vc-shell=2.6.0; store=B2B-store; role=admin
    at: 2026-10-12T19:06:14.661Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-12, MutationObserver on the notification list, 8 s per action, own AGENT-TEST pages (all archived): Publish 0/3 toasts, Unpublish 0/3, Archive 0/4 (requests 200/204); Save 'Page saved successfully' (2/2), Clone 'Page cloned successfully' at ~650-820 ms, visible after the swap (2/2), Load content 'Content loaded successfully' (1/1), Save of an imported page 'Page saved successfully' (1/1). Source: PageDetails.vue notification.success only for save/download/clone at tags 3.1033.0 and 3.1034.0."
    investigation: true
    bodyHash: cb8083f4a0be
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_stable
    build: b0005569472f16df
    conditions: platform=3.1039.15; module:VirtoCommerce.PageBuilderModule=3.1022.0; store=B2B-store; role=admin
    at: 2026-10-19T09:54:51.183Z
    by: session:6ac5ef0c
    bodyHash: cb8083f4a0be
    defect: VCST-4973
    who: Lenajava1
    note: "2026-10-19, Edge, own AGENT-TEST pages (all archived), snapshots at ~0 / +1.5 / +4 s per action: Publish 0/4 toasts (POST /grouped/publishing/{id}?publish=true -> 200), Unpublish 0/2 (?publish=false -> 200), Archive of a Published page 0/2 and of a Draft page 0/2 (POST /grouped/archive?ids= -> 204, blade closes). Save showed 'Page saved successfully' every time (5/5). The shell notification centre (bell) read 'No notifications yet' afterwards. Same shell layout as 3.1033.0 (toolbar Save/Archive/Open designer/Save content/Clone/Publish|Unpublish)."
---
On released PageBuilderModule builds (observed live on 3.1033.0 with vc-shell 2.6.0 on vcst, 2026-10-12, administrator, page details blade opened from the store's Page Builder widget): Publish (POST /api/page-builder-pages/grouped/publishing/{groupId}?publish=true -> 200, 3 runs), Unpublish (?publish=false -> 200, 3 runs) and Archive (Confirmation 'Are you sure you want to archive this page?' -> Confirm -> POST /grouped/archive?ids= -> 204, 4 runs) all succeed, but no notification is rendered within 8 s; the only signals are the status chip and the Publish/Unpublish button swapping (Archive closes the blade). Save shows 'Page saved successfully', Clone shows 'Page cloned successfully' once and it stays visible after the blade swaps to the copy (2/2), Load content shows 'Content loaded successfully', and saving an imported page shows 'Page saved successfully' (Save stays disabled after an import until a field changes). The toast is a role=status item in the top-level notification list with a 'Dismiss notification' button. Source: PageDetails.vue at tags 3.1033.0 and 3.1034.0 calls notification.success only for SAVE_SUCCESS, DOWNLOAD_SUCCESS and CLONE_SUCCESS; the publish/unpublish/archive handlers have none (dropped in the @vc-shell 2.0.3 upgrade, VCST-5105). The unmerged PR VirtoCommerce/vc-module-pagebuilder#172 (VCST-4973, build 3.1034.0-pr-172-5aad) restores 'Page published successfully', 'Page unpublished successfully' and 'Page archived successfully'.
