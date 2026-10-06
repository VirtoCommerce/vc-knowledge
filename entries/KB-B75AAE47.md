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
---
Storefront 2.59.0-pr-2519, UTC+3, signed in as a sales rep: Filters -> Created date "Last week" -> Apply sends createddate:["<today-7 local midnight>" TO "<today 23:59:59.999 local>"] and shows Start/End chips. Removing the Start chip sends one SalesRepCustomerOrders request with createddate:[TO "<today 23:59:59.999 local>"] (all historical orders return), but reopening the drawer shows the Created date combobox still reading "Last week", no date fields, and Apply disabled. The drawer never reflects the open-ended filter actually applied. Removing one chip of a Custom date range instead behaves consistently (the drawer shows the remaining bound).
