---
id: KB-255E2DDB
subject: "Site Builder Vue designer protects a template's main section: Hide and Remove are disabled, but multi-select Delete confirms and then silently removes nothing"
plane: experiential
question: Can the main section of a template be hidden or removed in the Site Builder Vue designer?
questions:
  - text: What happens when you multi-select the main section and press Delete in the Site Builder designer?
  - text: Is the Product info / main section of the product template removable in the Vue designer?
  - text: Why does Remove 1 section(s) confirm but nothing is removed in the Site Builder designer?
status: active
appliesTo:
  - axis: feature
    value: site-builder-designer
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/site-builder-shell#/site-builder-draft/site-builder-designer
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; platform=3.1099.0; template=product Default
    defect: suspected
    at: 2026-11-19T13:32:41.961Z
    by: session:51ac2fe3
    who: Lenajava1
---
On a store whose preview loads, opened the Product template (Default) in the Vue designer. The main section row (Product) shows Hide disabled; its More actions menu offers only Duplicate/Copy/Paste (no Hide/Remove); the settings panel's Remove section button is disabled with the note "Every template keeps its main section: it can't be removed or hidden." Via Sections > Select, ticking the main section enables Delete ("1 selected"); Delete opens "Remove 1 section(s) from the page?"; Confirm closes the dialog, the section stays, selection resets to 0, Save/Undo stay disabled and no message is shown. The protection holds; the multi-select path offers and confirms an action it then silently drops. Same on a store without a loading preview (seen earlier in the day).
