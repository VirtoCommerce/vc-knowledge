---
id: KB-C466AD5C
subject: Barcode scanner blade without BrowseFilters:Read shows the scanner as off
plane: experiential
question: What does the Store > Search configuration > Barcode scanner blade show to a user without catalog:BrowseFilters:Read?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: GET /api/catalog/barcode-search/store
  - coordinate: /workspace/store
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T14:12:01.051Z
    by: session:p43300
    who: Lenajava1
---
Admin SPA, Catalog 3.1046 (PR 909 build): the tile still opens; the blade calls only GET .../fields, gets 403, shows an 'Error 403' banner, and still renders the switch OFF with neither match mode selected, although the stored value is scannerEnabled=true. The settings GET is never issued. With Read but not Update the blade renders the stored state correctly: no Save, Reset disabled, rows and radios inert.
