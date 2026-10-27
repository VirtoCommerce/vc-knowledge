---
id: KB-E1BFA8C8
subject: Reloading the browser on an Admin Shell page-builder designer URL shows an empty page with 'Has unsaved changes', and Save then sends nothing
plane: experiential
question: What happens when the Admin Shell page designer is reloaded on its own URL?
questions:
  - text: Does the Admin Shell page builder designer survive a browser refresh?
  - text: Can saving after a designer reload overwrite a content page with an empty one?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/content/page-builder-designer
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: fcd091cc4597b860
    conditions: platform=3.1090.0-alpha.13438; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-34b3; framework=@vc-shell/framework 2.6.3-rc.0
    defect: suspected
    at: 2026-10-27T11:31:04.201Z
    by: session:5de4e186
    who: kutasinaelena
---
Duplicated a content page (Sites > B2B-store > Files > Pages, row action Duplicate then Rename) and opened it from the list: designer showed title Home, 4 sections (Login hidden, Slider, Products, Features), Save disabled. Full browser reload on the designer URL (#/content/page-builder-designer?storeId=...&path=/<name>.page-draft&type=pages): blank title, 0 sections, 'Has unsaved changes' banner and 'Unsaved changes' badge, Save enabled; no page-content request appeared in the network log. Pressing Save produced no API request; the list afterwards still showed the same size (1645) and modified time, and reopening from the list showed the 4 sections again. Leaving the reloaded designer triggered a beforeunload prompt.
