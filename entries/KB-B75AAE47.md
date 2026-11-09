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
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; culture=de-DE
    at: 2026-10-29T00:01:05.245Z
    by: session:ce3cfc2a
    bodyHash: 617d55a11041
    who: Aleksandra-Mitricheva
    note: "vcst_qa, de-DE, customer-scoped route: Letzter Monat -> chips Beginn 29.09.2026 / Ende 29.10.2026; removing Beginn sent one request createddate:[TO \"2026-10-29T20:59:59.999Z\"]; drawer still 'Letzter Monat', no date fields, Apply disabled."
  - method: observation
    deployment: vcptcore_qa
    build: 9ad05b08a6408392
    conditions: module:VirtoCommerce.SalesRep=3.1020.0-pr-35-08fd; theme=2.62.0-alpha.2628; culture=de-DE
    at: 2026-11-09T13:41:59.361Z
    by: session:70c29718
    bodyHash: 617d55a11041
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.62.0-alpha.2628 vs 2.61.0-alpha.2605; theme 2.62.0-alpha.2628 vs 2.61.0-alpha.2605; virtocommerce.customer 3.1032.0-pr-324-b30b vs 3.1031.0; virtocommerce.pages 3.1013.0-alpha.93-vcst-6442 vs 3.1011.0; virtocommerce.xapi 3.1029.0 vs 3.1028.0; virtocommerce.xcart 3.1044.0 vs 3.1042.0; …
    note: "Fixed on SalesRep 3.1020.0-pr-35-08fd (2026-11-09, de): after applying 'Letzte Woche' (chips Beginn 02.11.2026 / Ende 09.11.2026), removing the Beginn chip and reopening the drawer shows 'Benutzerdefiniertes Datum' with an empty Startdatum and Enddatum 09.11.2026. Apply is disabled until something changes (the drawer matches the applied filter); picking a start date enables it, and Apply gives chips 03.11–09.11. Defect-fixed candidate: fixedIn VirtoCommerce.SalesRep@3.1020.0-pr-35-08fd (a PR build)."
---
Storefront 2.59.0-pr-2519, UTC+3, signed in as a sales rep: Filters -> Created date "Last week" -> Apply sends createddate:["<today-7 local midnight>" TO "<today 23:59:59.999 local>"] and shows Start/End chips. Removing the Start chip sends one SalesRepCustomerOrders request with createddate:[TO "<today 23:59:59.999 local>"] (all historical orders return), but reopening the drawer shows the Created date combobox still reading "Last week", no date fields, and Apply disabled. The drawer never reflects the open-ended filter actually applied. Removing one chip of a Custom date range instead behaves consistently (the drawer shows the remaining bound).
