---
id: KB-BC55C4F0
subject: barcode-search endpoints are gated by catalog:BrowseFilters:Read (GET) and :Update (PUT)
plane: experiential
question: What permissions do the barcode-search settings endpoints require?
questions:
  - text: Which permission lets a back-office manager view the store's barcode scanning settings?
  - text: What does a read-only admin role get when trying to save barcode search configuration over the API?
  - text: Which catalog permissions gate GET and PUT on the barcode-search store endpoints?
  - text: Do store access or catalog access permissions alone allow reading barcode search settings?
  - text: What status does an anonymous caller receive from the barcode search settings endpoints?
concepts:
  - id: barcode-settings
  - id: permission
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/catalog/barcode-search/store/{storeId}
  - coordinate: GET /api/catalog/barcode-search/store/{storeId}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T13:30:11.826Z
    by: session:p51696
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-28T14:12:30.870Z
    by: session:p43332
    who: Lenajava1
    note: "Read-only role: GET 200, GET fields 200, PUT 403 (unchanged); no-permission role: 403 on all three; anonymous 401."
  - method: observation
    deployment: vcst
    at: 2026-09-28T16:03:59.351Z
    by: session:p44044
    who: Lenajava1
    note: "Read-only role: GET 200, PUT 403, stored value unchanged; no-permission role: GET, GET fields and PUT all 403."
---
For a non-administrator back-office (Manager) user: GET /api/catalog/barcode-search/store/{storeId} and GET .../fields return 200 with catalog:BrowseFilters:Read and 403 without it; PUT /api/catalog/barcode-search/store/{storeId} returns 403 when the user holds catalog:BrowseFilters:Read but not catalog:BrowseFilters:Update. store:access, store:read and catalog:access alone grant none of the three.
