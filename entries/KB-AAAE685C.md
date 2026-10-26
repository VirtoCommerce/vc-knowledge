---
id: KB-AAAE685C
subject: Admin Shell Menu Editor discards unsaved changes without a confirmation when the user navigates away (rail item, Ctrl+K search, hash change)
plane: experiential
question: Does the Admin Shell Menu Editor ask to confirm before leaving with unsaved changes?
questions:
  - text: Are unsaved Admin Shell menu edits lost when navigating to another screen?
  - text: Which confirmations does the Admin Shell Menu Editor show?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.adminshell
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/menu-editor
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=administrator; browser=edge
    defect: suspected
    at: 2026-10-23T09:20:17.103Z
    by: session:5de4e186
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=administrator; browser=edge
    at: 2026-10-23T12:55:30.213Z
    by: session:5de4e186
    bodyHash: 238707002d5b
    who: kutasinaelena
    note: "Re-observed with a rename (row chip Changed): leaving via rail Products > Catalogs and via browser Back discards silently; a full document unload shows NO beforeunload dialog either (unlike the Settings screen). No close (X) on the root blade."
  - method: observation
    deployment: vcptcore_qa1
    build: 3c977d7c206bc258
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-34b3; role=administrator; browser=chromium
    at: 2026-10-26T09:33:40.932Z
    by: session:5de4e186
    bodyHash: 238707002d5b
    who: kutasinaelena
    contradicts: true
    contested: module:VirtoCommerce.AdminShell 3.1000.0-pr-1-34b3 vs 3.1000.0-pr-1-f481; browser chromium vs edge
    note: On AdminShell 3.1000.0-pr-1-34b3, with one item hidden and unsaved, leaving via the rail home button and via browser Back opened "You have unsaved changes. Close anyway?" (Cancel kept the change), and a full document navigation raised the native beforeunload dialog. Reset discarded the change; nothing saved.
---
As administrator in the Menu Editor, hiding an item enables Save/Reset and marks the item Hidden. Then opening Dashboard from the Home panel, opening Orders via Ctrl+K search + Enter, or changing the URL hash to #/ each navigated immediately with no dialog; returning to the editor shows the change gone and GET /api/admin-shell/menu/global confirms nothing was stored. Undo all changes does show a Confirmation dialog ("Undo all changes to the menu? ...") with Confirm/Cancel, and Undo changes on one row reverts that row only.
