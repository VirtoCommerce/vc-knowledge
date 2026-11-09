---
id: KB-BE19E49D
subject: On a cold load of a non-default locale, the sales-rep orders filter drawer's Created date combobox shows the raw key sales_rep.customer_orders.filters.custom_date as its placeholder
plane: experiential
question: Does the Created date combobox in the sales-rep customer-orders filter drawer show a localized placeholder when the page is loaded directly in a non-English locale?
status: active
appliesTo:
  - axis: build
    value: 2.59.0-pr-2519
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/customer-orders
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T16:40:07.752Z
    by: session:7982e016
    who: Aleksandra-Mitricheva
    build: 2077d219738578a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: platform=3.1085.0; theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; culture=de
    at: 2026-10-19T23:36:35.543Z
    by: session:68b36f2b
    bodyHash: 818af6f828ad
    who: Aleksandra-Mitricheva
    note: "Cold load of /de/company/customer-orders on the plugin build: Created date combobox shows sales_rep.customer_orders.filters.custom_date although the plugin's de bundle carries the key (Benutzerdefiniertes Datum)."
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    conditions: theme=2.61.0-pr-2577; plugin:sales-rep=0.2.5
    at: 2026-10-27T12:31:52.245Z
    by: session:11cd46bd
    bodyHash: 818af6f828ad
    defect: VCST-6196
    who: Aleksandra-Mitricheva
    note: "vcst_qa: direct full load of /ja/ and /zh/company/customer-orders showed the raw key (1/1 each); /de/ direct load showed 'Benutzerdefiniertes Datum' 2/2 — de did not reproduce cold here."
  - method: observation
    deployment: vcptcore_qa
    build: 9ad05b08a6408392
    conditions: module:VirtoCommerce.SalesRep=3.1020.0-pr-35-08fd; theme=2.62.0-alpha.2628; browser=chromium
    at: 2026-11-09T13:41:58.247Z
    by: session:70c29718
    bodyHash: 818af6f828ad
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.62.0-alpha.2628 vs 2.61.0-pr-2577; theme 2.62.0-alpha.2628 vs 2.61.0-pr-2577-4c17-4c171a96; virtocommerce.customer 3.1032.0-pr-324-b30b vs 3.1030.0; virtocommerce.orders 3.1019.0 vs 3.1018.0; virtocommerce.pages 3.1013.0-alpha.93-vcst-6442 vs 3.1010.0; …
    note: "Fixed on SalesRep 3.1020.0-pr-35-08fd (2026-11-09, Chrome, sales rep): direct loads of /ja/, /zh/ and /de/company/customer-orders (de 3x, one in a new tab), then Filter, show '日付指定', '自定义日期' and 'Benutzerdefiniertes Datum'. The raw key never appeared. At 375 px in zh the label fits (input scrollWidth 250 = clientWidth 250, no page horizontal scroll). Defect-fixed candidate: fixedIn VirtoCommerce.SalesRep@3.1020.0-pr-35-08fd (a PR build)."
---
Storefront 2.59.0-pr-2519: logged in as a sales rep, loading /<locale>/company/customer-orders directly by URL (ja, zh, ru, pl, fr, es, it, pt, de; no in-session language switch) and opening Filters shows the Created date combobox with value/placeholder 'sales_rep.customer_orders.filters.custom_date'. The dropdown options are translated, and picking the Custom date option replaces the raw key with the translated label. It persists for the life of the page: reopening the drawer and resizing to 375px still shows the key, and at 375px the key overflows the input (scrollWidth 339 vs clientWidth 250). With the default locale (en, no prefix) the placeholder correctly reads 'Custom date'. This looks like the placeholder being resolved once, before the lazily loaded locale messages arrive. It extends KB-23CBD97B (in-session switch) to cold loads.
