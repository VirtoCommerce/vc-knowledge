---
id: KB-EDAE238D
subject: Designer asset picker New folder posts one POST /api/assets/folder with the current folder as parentUrl, validates the name client-side, then opens the new folder
plane: experiential
question: How does the New folder action in the Page Builder Designer asset picker behave and validate names?
questions:
  - text: What folder names does the Page Builder asset picker accept?
  - text: Does double-clicking Create in the asset picker create two folders?
  - text: Who sees New folder and Upload in the Page Builder Designer asset picker?
status: active
appliesTo:
  - axis: surface
    value: admin-pagebuilder-designer
anchors:
  - coordinate: POST /api/assets/folder
  - coordinate: page-builder-designer/index.html#/pages
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7c3298a355b90935
    conditions: platform=3.1076.0; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-166-e2d3
    at: 2026-10-23T08:46:46.539Z
    by: session:153e3820
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: e68e6af5958b13d2
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-166-3a31
    at: 2026-10-26T08:37:19.735Z
    by: session:153e3820
    bodyHash: ab6a9f56427a
    who: kutasinaelena
    note: "Re-observed on 3a31 build: the form now has icon buttons 'Create folder' (check) and 'Cancel new folder' (close) in the input row, DOM/Tab order input -> Create folder -> Cancel new folder; Create disabled for empty/invalid; fast double-click = one POST 204; Esc/Cancel close only the form and return focus to New folder. Permission gating re-confirmed with restricted roles with and without platform:asset:create."
  - method: observation
    deployment: vcst_qa
    build: 8d4bda9222c60036
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-166-4108
    at: 2026-10-26T10:54:57.889Z
    by: session:153e3820
    bodyHash: ab6a9f56427a
    who: kutasinaelena
    note: One POST /api/assets/folder 204, picker entered the new folder, 2 pending multi-select picks kept ('2 selected'), Select applied both.
---
In the Designer "Choose from asset library" picker, New folder opens an inline form (focus goes to the input). Names are validated before any request: 3-63 chars, lowercase letters, numbers, spaces and single dashes, no leading dash, no consecutive dashes; invalid input shows an inline alert ("must only contain lowercase letters...", "at least 3 characters", "at most 63 characters", "must not start with a dash", "must not contain consecutive dash symbols") and Create is disabled, Enter sends nothing. A valid name sends exactly one POST /api/assets/folder {name, parentUrl:<current folder relative url>} -> 204 even on a fast double-click, then the picker navigates into the new folder, the breadcrumb gains it and the search keyword is cleared. Enter submits; Esc and Cancel close only the form and discard the draft. parentUrl segments such as {{draft}} are sent and rendered literally. New folder and Upload are shown only to users with platform:asset:create; a user without it sees neither, and a file dropped on the grid sends no upload.
