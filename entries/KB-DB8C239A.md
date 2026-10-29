---
id: KB-DB8C239A
subject: With storefront culture el (no Sales Rep plugin locale), the hub rail, dashboard and the buyer's Corporate 'Sales reps' link render in English with no raw i18n keys; de and es render translated labels
plane: experiential
question: What do the Sales Rep hub rail and the Corporate Sales reps link show in Greek (el), a culture the plugin ships no locale for?
questions:
  - text: Does the sales rep hub fall back to English when the storefront language has no plugin translation?
  - text: What are the German and Spanish labels of the Sales Rep hub rail and the Corporate Sales reps link?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /account/dashboard
  - coordinate: /company/sales-reps
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; module:VirtoCommerce.SalesRep=3.1016.0; store=B2B-store
    at: 2026-10-29T00:27:57.611Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
    note: "distinct: This is the SETTLED per-culture label set (de/es translated, el falls back to English), not the transient raw-key window while the locale chunk loads (KB-AEBE2951) nor the non-rep redirect (KB-ACD62417)."
---
B2B-store, language switched through the header selector (full reload to /<culture>/...). Rep: en rail heading 'Sales Rep hub' (Dashboard, My customers, Tasks, My activity); de 'Vertriebsmitarbeiter-Hub' (Dashboard, Meine kunden, Aufgaben, Meine aktivität); es 'Centro del representante de ventas' (Panel de control, Mis clientes, Tareas, Mi actividad), dashboard H1 'Panel de control'; el: rail, dashboard and whole account page stay English ('Sales Rep hub', H1 'Dashboard'). Buyer Corporate link: en 'Sales reps', de 'Vertriebsmitarbeiter', es 'Representantes de ventas', el 'Sales reps'. No sales_rep.* or UPPER.DOTTED key in any settled frame. Each language switch reloads the page; the settled state needs a few seconds.
