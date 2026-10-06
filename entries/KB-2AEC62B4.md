---
id: KB-2AEC62B4
subject: "Barcode scanner blade: Exact-match mode reveals a 'Fields to match *' checklist; Reset reverts unsaved mode/field changes with no PUT"
plane: experiential
question: What does the Admin Barcode scanner blade show in each match mode, and does Reset discard unsaved changes without writing?
questions:
  - text: What extra options appear in the store barcode scanner settings when exact matching is chosen?
  - text: GET /api/catalog/barcode-search/store/{}/fields Fields to match checklist sorting
  - text: Does Reset on the Barcode scanner blade discard unsaved match-mode changes without a PUT?
  - text: How are built-in fields like GTIN, MPN and SKU labelled in the Fields to match list?
  - text: Which fields carry a MULTI-VALUE badge in the barcode scanner blade?
concepts:
  - id: barcode-settings
  - id: admin-validation
status: active
appliesTo:
  - axis: blade
    value: store-barcode-scanner
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/catalog/barcode-search/store/{}
  - coordinate: GET /api/catalog/barcode-search/store/{}/fields
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-01T16:39:36.545Z
    by: session:1d6c15f5
    who: Lenajava1
---
Platform 3.1075.0, store Search configuration > Barcode scanner blade, stored {scannerEnabled:true, fields:[]}. Full-text mode shows: heading 'Choose how a scanned code is matched against products', switch 'Enable barcode scanner in the storefront', radios under 'Match scanned code by' ('Full-text search' / 'Exact match on selected fields'), a 'How match modes work' box and a 'How to fill barcode data' box; toolbar Save and Reset both disabled. Selecting 'Exact match on selected fields' inserts a 'Fields to match *' checklist (87 index fields on this store) between the two boxes; rows are sorted alphabetically by displayed label, built-ins show a label plus field name (GTIN gtin, MPN manufacturerPartNumber, SKU code), and multi-value properties carry a MULTI-VALUE badge. Save and Reset become enabled on change. Clicking Reset returned the blade to Full-text with the checklist hidden and both buttons disabled; network showed only the initial GET store/{id} and GET store/{id}/fields, no PUT, and a re-GET after Reset returned the same body.
