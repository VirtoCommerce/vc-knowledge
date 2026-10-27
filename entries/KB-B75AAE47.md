---
id: KB-B75AAE47
subject: Removing one chip of an applied date preset on the sales-rep orders page leaves the filter drawer still showing the preset name while the grid filters by the remaining open-ended bound
plane: experiential
question: After applying a Created date preset (e.g. Last week) on /company/customer-orders and removing its Start chip, what does the request send and what does the Filters drawer show?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/customer-orders
  - coordinate: Query.salesRepCustomerOrders
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T14:52:30.035Z
    by: session:7982e016
    who: Aleksandra-Mitricheva
    build: 2077d219738578a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: platform=3.1085.0; theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc
    at: 2026-10-19T23:36:34.990Z
    by: session:68b36f2b
    bodyHash: 617d55a11041
    who: Aleksandra-Mitricheva
    note: "Re-observed with the Last month preset: removing the Start chip sent createddate:[TO \"<today 23:59:59.999 local>\"], reopened drawer still read \"Last month\" with no date fields and Apply disabled."
  - method: observation
    deployment: vcptcore_qa
    build: 956cfff4cb0a606f
    at: 2026-10-27T11:14:54.465Z
    by: session:37167127
    bodyHash: 617d55a11041
    defect: VCST-6195
    who: Aleksandra-Mitricheva
    note: "Re-observed 2026-10-27 with Last week: Apply sent createddate:[\"2026-10-19T21:00:00.000Z\" TO \"2026-10-27T20:59:59.999Z\"], removing Start sent one request with createddate:[TO \"2026-10-27T20:59:59.999Z\"]; drawer still \"Last week\", no date fields, Apply disabled. The page code is the SalesRep module's storefront plugin chunk, so a theme-only build does not change it."
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; theme=2.61.0-pr-2576-be2c; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; culture=de-DE
    at: 2026-10-27T20:15:23.559Z
    by: session:e75fe8a4
    bodyHash: 617d55a11041
    who: Aleksandra-Mitricheva
    note: "Re-observed with 'Letzter Monat' on the customer-scoped route under de-DE: one request createddate:[TO end-of-today], drawer still 'Letzter Monat'."
---
Storefront 2.59.0-pr-2519, UTC+3, signed in as a sales rep: Filters -> Created date "Last week" -> Apply sends createddate:["<today-7 local midnight>" TO "<today 23:59:59.999 local>"] and shows Start/End chips. Removing the Start chip sends one SalesRepCustomerOrders request with createddate:[TO "<today 23:59:59.999 local>"] (all historical orders return), but reopening the drawer shows the Created date combobox still reading "Last week", no date fields, and Apply disabled. The drawer never reflects the open-ended filter actually applied. Removing one chip of a Custom date range instead behaves consistently (the drawer shows the remaining bound).
