---
id: KB-D0BF25FF
subject: Barcode scanner blade keeps field order fixed while editing and asks before closing with unsaved changes
plane: experiential
question: How does the Admin SPA Barcode scanner blade order fields and guard unsaved changes?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: PUT /api/catalog/barcode-search/store/{}
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T16:04:18.280Z
    by: session:p35172
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:27:17.311Z
    by: session:p42296
    who: Lenajava1
    contradicts: true
    note: "Order frozen while toggling, Exact with zero fields disables Save, Reset restores and disables Save, dirty close opens 'Save changes' (Yes/No) and No discards - all as stated. But the saved fields are NOT shown in stored order: GET returned fields [code, gtin] and the reopened blade listed GTIN then SKU (alphabetical by label). Also: closing while dirty AND invalid (Exact, zero fields) opens a different 'Warning - Validation failed for this object. Will you continue editing and save later?' dialog; No closes the blade without saving."
---
Catalog 3.1046 (PR 909 build): the blade lists the saved (checked) fields first in stored order, then every other field sorted by its display label (GTIN, MPN and SKU sort by label, e.g. MPN between monday and mycustom). The order does not change while the admin toggles modes or checkboxes. In Exact mode with zero fields selected Save is disabled. Reset restores the saved selection and mode and disables Save. Closing the blade with unsaved changes opens a 'Save changes' dialog (Yes / No); No discards. After toggling the mode away and back, Save stays enabled although the form equals the saved state.
