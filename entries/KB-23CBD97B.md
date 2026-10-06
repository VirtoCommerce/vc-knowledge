---
id: KB-23CBD97B
subject: After an in-session language switch, the sales-rep orders filter drawer shows the raw i18n key sales_rep.customer_orders.filters.custom_date as the selected Created date value
plane: experiential
question: Does the Created date combobox in the sales-rep orders filter drawer localize after switching the storefront language?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/customer-orders
  - coordinate: /company/my-customers/:organizationId/orders
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T14:52:30.074Z
    by: session:7982e016
    who: Aleksandra-Mitricheva
---
Storefront 2.59.0-pr-2519: on /company/customer-orders or /company/my-customers/<org>/orders, switching language via the header switcher (en->de, de->fr) and then opening Filters shows the Created date combobox value as the raw key "sales_rep.customer_orders.filters.custom_date". The listbox options themselves are translated ("Benutzerdefiniertes Datum"), and selecting an option fixes the label. A direct page load in that language (/de/company/customer-orders) shows the translated value, so only the initially selected value captured before the new locale messages load is affected. Reproduced 2/2.
