---
id: KB-30B5E2BD
subject: On vcst_dev the generic export registry lists no order type and the Admin Orders list blade has no Export action; GET /api/import/providers is 404
plane: experiential
question: Is there an order export provider in the Admin, and which endpoint lists export providers?
questions:
  - text: Does the Admin Customer orders blade have an Export toolbar button?
  - text: What does GET /api/import/providers return?
  - text: Which types does GET /api/export/knowntypes list?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/export/knowntypes
  - coordinate: GET /api/import/providers
evidence:
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    conditions: platform=3.1100.0
    at: 2026-11-18T17:34:31.504Z
    by: session:feee22e7
---
2026-11-18, admin session cookie: GET /api/export/knowntypes returned 200 with exactly five exportable types — ExportableCatalogFull, ExportableProduct (Catalog) and ExportablePricelist, ExportablePrice, ExportablePricelistAssignment (Pricing); no order type. GET /api/import/providers returned 404 (no such route on this build). The Admin 'Customer orders' list blade toolbar shows only Refresh, Delete and Send to AvaTax — no Export action. A check expecting an order export provider via /api/import/providers is testing a route this platform does not expose.
