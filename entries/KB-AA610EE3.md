---
id: KB-AA610EE3
subject: Barcode scanner blade shows the scanner OFF with no match mode whenever GET .../fields fails, not only on 403
plane: experiential
question: What does the Admin SPA Barcode scanner blade show when api/catalog/barcode-search/store/{id}/fields fails?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: GET /api/catalog/barcode-search/store/{id}/fields
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:27:32.413Z
    by: session:p32560
    who: Lenajava1
---
Catalog 3.1046 (PR 909 build), store Search configuration > Barcode scanner. On open the blade first calls GET api/catalog/barcode-search/store/{id}/fields. When that request fails at the transport level (browser status -1, net::ERR_CONNECTION_CLOSED) the blade shows an 'Error -1' banner and renders the switch OFF with neither Full-text nor Exact selected and no field list, although the stored value is scannerEnabled=true; the settings GET is not issued. This is the same false 'off' rendering as the 403 case for a user without catalog:BrowseFilters:Read, so the false state follows any failure of the fields call, not the permission specifically. Closing and reopening the blade after the network recovered rendered the stored state correctly.
