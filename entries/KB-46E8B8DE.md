---
id: KB-46E8B8DE
subject: The buyer account dashboard shows four live order stat cards and a My recent orders widget; the old Latest orders / Monthly spend report / Orders status widgets are gone
plane: experiential
question: What does the buyer account dashboard /account/dashboard show — which stat cards and widgets, and where do the figures come from?
questions:
  - text: Which stat cards does /account/dashboard render (orders placed week MTD YTD, avg order value)?
  - text: Which GraphQL operations does the buyer account dashboard call?
  - text: Are the Latest orders, Monthly spend report and Orders status widgets still on the account dashboard?
  - text: Does the account dashboard save a per-user layout?
status: active
appliesTo:
  - axis: role
    value: b2b-buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/dashboard
  - coordinate: Query.orderStatistics
  - coordinate: Query.layout
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: c185459e7260c4a3
    conditions: theme=2.62.0-pr-2552-ec75; module:VirtoCommerce.XFrontend=3.1010.0-pr-12-3ca5; module:VirtoCommerce.Orders=3.1020.0-pr-507-d3ef; module:VirtoCommerce.SalesRep=3.1019.0-pr-24-2fd6; platform=3.1094.0; role=Organization maintainer
    at: 2026-11-09T09:03:34.299Z
    by: session:9fb93ecf
    who: kutasinaelena
    note: "distinct: KB-EBAE1172 records the old Monthly spend report widget on builds before VCST-6078; this entry records the replacement dashboard on the VCST-6078 PR build, where that widget no longer renders."
  - method: observation
    deployment: vcptcore_qa1
    build: c185459e7260c4a3
    conditions: theme=2.62.0-pr-2552-ec75; module:VirtoCommerce.XFrontend=3.1010.0-pr-12-3ca5; platform=3.1094.0
    at: 2026-11-09T09:57:37.040Z
    by: session:9fb93ecf
    bodyHash: 2a0a4bb07188
    who: kutasinaelena
    note: "Chrome lane, John/Sarah/BD_* buyers: same four cards + My recent orders (chips incl. Cancelled, filter works), Edit layout with Reset/Cancel/Save; GetLayout null for a never-saved user; empty org shows 0 / $0.00 and 'There are no orders yet' + 'Continue browsing'."
---
Signed in as a B2B org maintainer on the VCST-6078 PR build, /account/dashboard renders four stat cards — "Orders placed · WEEK", "Orders placed · MTD", "Orders placed · YTD" (order count as the big number, total amount as the sub-line, a signed "% vs last …" change when the previous period has orders; none when it is empty) and "Avg order value" ("Average per order (YTD)", no change line) — plus a "My recent orders" widget (5 rows, status chips with counts, "All orders" link) and an "Edit layout" button (edit mode with Reset / Cancel / Save layout). The previous widgets Latest orders, Monthly spend report and Orders status do not render. On load the page calls GraphQL GetLayout (scope accountDashboard; returns layout null for a user who never saved), GetOrderStatistics and GetOrders (first 5 with a status facet), all 200 with no errors[]. A user in an organization with no orders sees all cards at 0 / $0.00 and the empty state "There are no orders yet" with "Continue browsing". The status chip counts include cancelled orders while the cards exclude them (chips Completed 17 + Cancelled 13 + New 13 + Processing 6 = 49; YTD card 36). This is an unmerged PR build (vc-frontend#2552 with vc-module-x-frontend#12), not a release.
