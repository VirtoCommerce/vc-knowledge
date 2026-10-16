---
id: KB-EA4C1F30
subject: "Dashboard layout reconcile and save always derive regions from the registry, and the dashboard registry now has a mainRight rail (tasks, plus documents for document-read reps), so the rail always renders and a save never emits mainRight: []"
plane: experiential
question: What regions and blocks does SaveSalesRepLayout emit for scope dashboard, and how are planted unknown or misplaced blocks reconciled?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: Mutation.saveSalesRepLayout
  - coordinate: Query.salesRepLayout
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb
    at: 2026-10-14T14:48:04.495Z
    by: session:6630b76f
    who: Lenajava1
---
Signed in as a sales rep on /company/dashboard (theme 2.60.0-alpha.2579). Save layout sends SaveSalesRepLayout with scope dashboard, schemaVersion 1 and regions in fixed order statistics (6 stat cards, settings []), mainLeft (orders, top_sellers, each settings [{key:maxRows,value:5}]) and mainRight (tasks with maxRows 5; a rep with sales-rep-documents:read also has documents with maxRows 5). Documents planted through the API with the rep's own token: unknown block types (legacy_activity_feed in statistics, legacy_news_panel in mainLeft) are kept by the server in the read response and silently dropped by the client, with no console error. Blocks stored in the wrong region render in their registry region (orders stored under statistics renders as the My recent orders widget; my_customers stored under mainLeft renders as a stat card; top_sellers stored under mainRight renders in mainLeft). Within the stats row the order is: document survivors, then re-homed blocks, then missing blocks in registry order. A no-change Save rewrites the stored document to the registry structure (unknown types removed, top_sellers back under mainLeft, mainRight = [tasks]). Hiding the My recent orders widget and saving stops SalesRepOrders, SalesRepOrderSortRules and SalesRepOrderFilterRules on reload, while a parked stat card's SalesRepCustomerOrderStatistics still fires. A double-click on Save layout sends exactly one mutation.
