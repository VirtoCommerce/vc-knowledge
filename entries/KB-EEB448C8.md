---
id: KB-EEB448C8
subject: Designer toolbar Publish and Unpublish give no toast; while a saved draft differs from the published page, Unpublish is disabled in the Designer and hidden in the shell blade
plane: experiential
question: What feedback does the Page Builder Designer toolbar give for Publish and Unpublish, and can a page whose draft has unpublished changes be unpublished from the UI?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: role
    value: administrator
  - axis: surface
    value: admin-designer
anchors:
  - coordinate: POST /api/page-builder-pages/grouped/publishing/{groupId}
  - coordinate: /Modules/$(VirtoCommerce.PageBuilderModule)/Content/page-builder-designer/index.html
  - coordinate: /apps/page-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T14:06:31.536Z
    by: session:f1cfecd7
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:38:35.307Z
    by: session:f1cfecd7
    who: Lenajava1
    note: "Same on PageBuilderModule 3.1034.0-pr-172-5aad (the shell-toast fix build): Designer toolbar Publish (?publish=true -> 200) and Unpublish (?publish=false -> 200) rendered no toast/alert (polling wait for 'successfully' timed out at 5 s each); only the Publish/Unpublish button enabled-state swapped. PR #172 restores toasts only in the shell page-details blade, not in the Designer."
---
On PageBuilderModule 3.1034.0-pr-167-0ed1 as administrator, the Designer toolbar Publish (POST /api/page-builder-pages/grouped/publishing/{groupId}?publish=true → 200) and Unpublish (?publish=false → 200) render no toast or alert in snapshots taken from the click to +15 s. The only change is the button state: after Publish, Publish is disabled and Unpublish enabled, and the reverse after Unpublish. Designer Save does show 'Template <page name> saved successfully' for about 4–5 s. While the editor has unsaved edits, both Publish and Unpublish are disabled. After a draft is saved over a published page, Unpublish stays disabled in the Designer, and the shell page-details blade shows chips 'Published' + 'Has draft with changes' with only Publish in its toolbar (Unpublish is visible only when publish-status hasChanges is false). So the server's refusal to unpublish a page that has changes cannot be reached from either UI surface. Source on the dev branch agrees: the Designer publishTemplate$/unpublishTemplate$ effects dispatch no showNotification on success, and an unpublish failure only dispatches getTemplatePublishStatusFails, which is silent.
