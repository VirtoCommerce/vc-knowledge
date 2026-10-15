---
id: KB-AEBE2951
subject: While the Sales Rep plugin's locale chunk is pending, the Corporate link and hub labels render raw i18n keys (e.g. "Sales_rep.navigation.link", "SALES_REP.HUB.TITLE", "PAGE.TITLE", "sales_rep.table.name")
plane: experiential
question: Does the storefront show raw sales_rep.* i18n keys before the Sales Rep plugin's translations load?
status: active
appliesTo:
  - axis: concern
    value: localization
  - axis: layer
    value: plugin-delivery
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /company/sales-reps
  - coordinate: /account/dashboard
  - coordinate: GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/assets/en-*.js
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; theme=2.60.0-alpha.2579; culture=en-US,es; fault=client-side request delay
    defect: suspected
    at: 2026-10-15T11:35:42.851Z
    by: session:6ee2ca49
    who: Lenajava1
---
Signed in as a sales rep who is also an org member. With plugin requests delayed client-side, the host-declared Corporate rail link renders the text "Sales_rep.navigation.link" from first paint until the plugin initialises (observed for 7+ s with /modules/** delayed 3 s per request, en and /es alike; es then localises to "Representantes de ventas"). With only the plugin locale chunk (plugins/vc-frontend/assets/en-*.js) delayed 6 s, the whole plugin UI mounts with raw keys: rail heading "SALES_REP.HUB.TITLE", items "Sales_rep.hub.dashboard.navigation.link", "Sales_rep.my_customers.navigation.link", "Sales_rep.tasks.title", page heading "PAGE.TITLE", dashboard widget keys such as sales_rep.orders.view_all; /company/sales-reps shows "PAGE.TITLE sales_rep.table.name sales_rep.table.email sales_rep.table.phone" above real rows — until the chunk arrives, then all labels localise. With remoteEntry.js delayed 12 s and no other delay, the Sales reps page still showed its raw table keys for about 0.25 s after mount. Clicking the raw-key Corporate link before the plugin arrives navigates to /company/sales-reps, shows "Loading page…", then the slow notice, then the real page.
