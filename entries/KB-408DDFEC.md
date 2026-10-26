---
id: KB-408DDFEC
subject: Page Builder Assets Library sort select offers Name A-Z/Z-A, Recently modified, Largest first (folders always first) and, with 'Sort by' as a prefix, has no accessible name
plane: experiential
question: How does the Sort by control in the Page Builder Assets Library workspace behave?
questions:
  - text: What sort options does the Page Builder Assets Library have?
  - text: Are folders sorted before files in the Page Builder Assets Library?
  - text: Does the Assets Library sort select have an accessible label?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/page-builder-assets/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: e68e6af5958b13d2
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-166-3a31
    defect: suspected
    at: 2026-10-26T08:37:20.233Z
    by: session:153e3820
    who: kutasinaelena
---
In the Page Builder shell Assets Library (#/page-builder-assets) the toolbar select shows 'Sort by' as an inline prefix and options Name (A-Z), Name (Z-A), Recently modified, Largest first. Every option keeps folders before files: A-Z/Z-A sort case-insensitively within each group; Recently modified orders by modifiedDate descending (matches GET /api/assets); Largest first sorts files by size descending and leaves folders A-Z. The select is 34px tall, level with the Search box (centres within 1px of the 36px buttons) at 1920/1280, and wraps to a second row with the view toggles at 768. In the accessibility tree the combobox has no accessible name and its listbox is named 'Field', because the component receives the text as prefix instead of label.
