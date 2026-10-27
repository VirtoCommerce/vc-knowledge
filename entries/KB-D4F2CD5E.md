---
id: KB-D4F2CD5E
subject: Entering Edit layout on /company/dashboard fetches SalesRepCustomerCartStatistics for a saved-hidden Active carts card, so the parked card shows live values in 'Hidden stats'
plane: experiential
question: When the Active carts stat card is saved as hidden, does its data load when the rep opens Edit layout on /company/dashboard?
questions:
  - text: Does a parked stat card in the Hidden stats zone show real values?
  - text: When does SalesRepCustomerCartStatistics fire if the Active carts card is hidden?
status: active
appliesTo:
  - axis: actor
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: Query.salesRepCustomerCartStatistics
  - coordinate: Query.salesRepLayout
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; theme=2.61.0-pr-2576-be2c; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; store=B2B-store
    at: 2026-10-27T20:59:20.345Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
    note: "distinct: KB-0AE6A961 covers the page mount (no fetch while hidden); this fact is what happens on entering edit mode (the query then fires and the parked card shows values)."
---
Signed in as a sales rep whose saved dashboard layout has active_carts hidden:true, a load of /company/dashboard sends no SalesRepCustomerCartStatistics. Clicking 'Edit layout' then issues SalesRepCustomerCartStatistics (cartFilter active-carts, store currency, weekFrom/weekTo) and the parked card under 'Hidden stats' renders its values (e.g. '10 items', '7 not for checkout', '0 items this week'). So a hidden stat card's query is deferred until edit mode, not skipped entirely.
