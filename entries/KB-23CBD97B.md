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
    build: 2077d219738578a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    conditions: theme=2.61.0-pr-2577; plugin:sales-rep=0.2.5; culture=de
    at: 2026-10-27T12:31:52.030Z
    by: session:11cd46bd
    bodyHash: 227876042ff6
    defect: VCST-6196
    who: Aleksandra-Mitricheva
    note: "vcst_qa 1/1: en -> Deutsch via header switcher on /company/customer-orders, Filter -> combobox 'Erstellungsdatum' reads the raw key."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; culture=de
    at: 2026-10-28T23:59:50.578Z
    by: session:ce3cfc2a
    bodyHash: 227876042ff6
    who: Aleksandra-Mitricheva
    note: en -> Deutsch via header switcher on /company/customer-orders, Filter drawer combobox 'Erstellungsdatum' reads the raw key; persists after applying a calendar-picked custom range and reopening the drawer.
  - method: observation
    deployment: vcptcore_qa
    build: 9ad05b08a6408392
    conditions: module:VirtoCommerce.SalesRep=3.1020.0-pr-35-08fd; theme=2.62.0-alpha.2628; browser=chromium
    at: 2026-11-09T13:41:57.472Z
    by: session:70c29718
    bodyHash: 227876042ff6
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.62.0-alpha.2628 vs 2.61.0-alpha.2605; theme 2.62.0-alpha.2628 vs 2.61.0-alpha.2605; virtocommerce.customer 3.1032.0-pr-324-b30b vs 3.1031.0; virtocommerce.xapi 3.1029.0 vs 3.1028.0; virtocommerce.xcart 3.1044.0 vs 3.1042.0; virtocommerce.xcms 3.1010.0-pr-28-35cb vs 3.1008.0; …
    note: "Fixed on SalesRep 3.1020.0-pr-35-08fd (2026-11-09, Chrome, sales rep): en->de via the header switcher on /company/customer-orders, then Filter, shows 'Benutzerdefiniertes Datum' 3/3 (never the raw key). de->fr shows 'Date personnalisée', fr->en shows 'Custom date', and the open listbox marks the current option aria-selected. Customer-scoped /de/company/my-customers/<id>/orders shows 'Benutzerdefiniertes Datum'. Defect-fixed candidate: fixedIn VirtoCommerce.SalesRep@3.1020.0-pr-35-08fd (a PR build, not yet a release)."
---
Storefront 2.59.0-pr-2519: on /company/customer-orders or /company/my-customers/<org>/orders, switching language via the header switcher (en->de, de->fr) and then opening Filters shows the Created date combobox value as the raw key "sales_rep.customer_orders.filters.custom_date". The listbox options themselves are translated ("Benutzerdefiniertes Datum"), and selecting an option fixes the label. A direct page load in that language (/de/company/customer-orders) shows the translated value, so only the initially selected value captured before the new locale messages load is affected. Reproduced 2/2.
