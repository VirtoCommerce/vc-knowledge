---
id: KB-E9DE91F6
subject: The storefront sales-rep customer profile fires two identical SalesRepCustomer queries per page mount
plane: experiential
question: How many salesRepCustomer requests does the storefront send when a sales rep opens a customer profile?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/:organizationId
  - coordinate: Query.salesRepCustomer
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; module:VirtoCommerce.SalesRep=3.1014.0-pr-13
    defect: suspected
    at: 2026-10-14T17:52:05.127Z
    by: session:6630b76f
    who: Lenajava1
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: platform=3.1085.0; theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc
    at: 2026-10-19T23:36:34.714Z
    by: session:68b36f2b
    bodyHash: 5270bfe96ccc
    who: Aleksandra-Mitricheva
    note: "Same on the module-delivered plugin build: SPA mount and full load each sent Query.salesRepCustomer twice with identical variables (once beside SalesRepLayout, once beside the block queries)."
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; theme=2.61.0-pr-2576-be2c; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4
    at: 2026-10-27T20:50:21.455Z
    by: session:e75fe8a4
    bodyHash: 5270bfe96ccc
    who: Aleksandra-Mitricheva
    note: Still twice per mount (row-click SPA mount and direct full load) after Sales Rep PR #26.
---
Opening /company/my-customers/{organizationId} as a sales rep (both via the My customers row link and via direct URL, and again on an SPA re-mount) sends Query.salesRepCustomer twice with the same organizationId per mount: once together with SalesRepLayout(scope customerProfile), and a second time after the layout resolves, alongside the block queries (SalesRepCustomerOrderStatistics, SalesRepOrders, SalesRepTopSellers...). Both return 200 with identical data and empty errors[]; the heading, Customer information card and rail render consistent values. So Apollo does not collapse the profile's salesRepCustomer consumers into one request on this build.
