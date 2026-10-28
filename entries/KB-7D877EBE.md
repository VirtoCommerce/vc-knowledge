---
id: KB-7D877EBE
subject: /account/orders Start-only date filter sends an open-ended createddate range with no upper bound, and End-only sends none for the lower
plane: experiential
question: What GetOrders filter does /account/orders send when only the Start date is applied in the Filters popover?
status: active
appliesTo:
  - axis: feature
    value: orders-date-filter
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders
  - coordinate: Query.orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; platform=3.1090.0; browser=firefox-utc+2
    at: 2026-10-28T10:54:22.189Z
    by: session:7e8ea7fb
    who: Lenajava1
---
On vcst_qa (theme 2.61.0-alpha.2605, browser UTC+2) applying only Start 10/13/2026 (End empty) on /account/orders fired GetOrders with filter createddate:["2026-10-12T22:00:00.000Z" TO] (open lower bound, nothing after TO). Start==End 10/13/2026 sent createddate:["2026-10-12T22:00:00.000Z" TO "2026-10-13T21:59:59.999Z"], i.e. local-day inclusive bounds converted to UTC. Chips shown: 'Start: 10/13/2026' only.
