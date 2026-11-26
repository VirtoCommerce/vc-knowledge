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
  - method: observation
    deployment: vcptcore_qa1
    build: c185459e7260c4a3
    conditions: theme=2.62.0-pr-2552-ec75
    at: 2026-11-09T10:35:03.548Z
    by: session:9fb93ecf
    bodyHash: 2a0a4bb07188
    who: kutasinaelena
    note: "Edge lane, BD_* buyers + USER2 + multi-org: same four cards, My recent orders with status chips, Edit layout (Reset / Cancel / Save layout); empty org 0 / $0.00 + 'There are no orders yet'; recent list includes a flag-cancelled order that the cards exclude."
  - method: observation
    deployment: vcptcore_qa1
    build: c185459e7260c4a3
    conditions: theme=2.62.0-pr-2552-ec75
    at: 2026-11-09T10:55:33.692Z
    by: session:9fb93ecf
    bodyHash: 2a0a4bb07188
    who: kutasinaelena
    note: "Firefox lane: non-rep buyer (Purchasing agent) and Organization maintainer hitting /company/dashboard are redirected to /account/dashboard, which shows the four cards + My recent orders, no Monthly spend; no SalesRep* GraphQL op issued."
  - method: observation
    deployment: vcptcore_qa1
    build: c185459e7260c4a3
    conditions: theme=2.62.0-pr-2552-ec75; store=B2B-store; role=Organization maintainer
    at: 2026-11-09T11:17:52.718Z
    by: session:9fb93ecf
    bodyHash: 2a0a4bb07188
    who: kutasinaelena
    note: "Regression run, Chrome: back navigation from /account/coupons to /account/dashboard re-renders the four Orders placed/Avg order value cards + My recent orders; no Latest orders or Monthly spend report; 0 console errors, 0 4xx/5xx."
  - method: observation
    deployment: vcptcore_qa
    build: 775727ee41d78e49
    conditions: theme=2.62.0-pr-2625-ac8f-ac8f78b4; store=B2B-store; role=Purchasing agent and Organization maintainer; browser=firefox
    at: 2026-11-11T07:11:27.654Z
    by: session:86a76b47
    bodyHash: 2a0a4bb07188
    who: Aleksandra-Mitricheva
    note: "Regression REG-2026-11-11-0702: both a Purchasing agent and an Organization maintainer on /account/dashboard see Orders placed WEEK/MTD/YTD + Avg order value, a 'Sales Statistics' widget (GetMonthlyOrderStatistics/GetOrderStatisticsHistory), My recent orders and Edit layout; no Latest orders / Monthly spend report."
  - method: observation
    deployment: vcst_qa
    build: fcea7ce160c179b6
    conditions: theme=vc-frontend-next-3.3.0-pr-23-7be3; store=B2B-store; browser=chromium
    at: 2026-11-16T12:57:42.759Z
    by: session:4cdfee7c
    bodyHash: 2a0a4bb07188
    who: Lenajava1
    note: "vc-frontend-next PR #23 build (merges vc-frontend 2.62.0), two buyers at 1280 and 375: four cards Orders placed WEEK/MTD/YTD + Avg order value, My recent orders (5 rows, status chips, All orders opens /account/orders), Edit layout with Reset/Cancel/Save layout. GetOrderStatistics/GetOrders/GetLayout 200 with no errors[]. Card figures equal GetOrderStatistics and the Orders list (cancelled order excluded from cards). A '+140% vs last month' line shows when the previous period has orders; null comparisons show no line. On this Next build the status chips carry no counts and no Sales Statistics widget renders."
  - method: observation
    deployment: virtostart
    build: 8f3b3fdd36b67c83
    conditions: theme=3.3.0; platform=3.1099.0; store=B2B-store
    at: 2026-11-17T12:06:03.680Z
    by: session:521b9a19
    bodyHash: 2a0a4bb07188
    who: Lenajava1
    note: "vc-frontend-next 3.3.0, B2B-store sales-rep org user with one order this year: four cards WEEK/MTD/YTD 1 / $899.98 + Avg order value, no change line (no previous-period orders), My recent orders with All/New chips (no counts), All orders opens /account/orders, Edit layout."
  - method: observation
    deployment: vcst_qa
    build: 957a892645884c54
    conditions: platform=3.1099.0; theme=2.63.0-pr-2667-0444-0444048a; store=B2B-store; role=Organization buyer
    at: 2026-11-20T10:51:18.349Z
    by: session:74ee3083
    bodyHash: 2a0a4bb07188
    who: Lenajava1
    note: Four cards and My recent orders as stated; on this build the status chips are All / New without counts, and the Sales Statistics block (KB-ADE54B03) renders between the cards and My recent orders.
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; store=B2B-store
    at: 2026-11-26T01:09:09.585Z
    by: session:779b94fc
    bodyHash: 2a0a4bb07188
    who: kutasinaelena
    note: "Regression REG-2026-11-26-0357-W2: four cards + My recent orders (5 rows, status tabs without counts, All orders -> /account/orders); a new order counted on the next load (12->13), a flag-cancelled order left all three cards immediately but stays in the list with status Cancelled; old Latest orders / Monthly spend report / Orders status absent in view and edit mode."
---
Signed in as a B2B org maintainer on the VCST-6078 PR build, /account/dashboard renders four stat cards — "Orders placed · WEEK", "Orders placed · MTD", "Orders placed · YTD" (order count as the big number, total amount as the sub-line, a signed "% vs last …" change when the previous period has orders; none when it is empty) and "Avg order value" ("Average per order (YTD)", no change line) — plus a "My recent orders" widget (5 rows, status chips with counts, "All orders" link) and an "Edit layout" button (edit mode with Reset / Cancel / Save layout). The previous widgets Latest orders, Monthly spend report and Orders status do not render. On load the page calls GraphQL GetLayout (scope accountDashboard; returns layout null for a user who never saved), GetOrderStatistics and GetOrders (first 5 with a status facet), all 200 with no errors[]. A user in an organization with no orders sees all cards at 0 / $0.00 and the empty state "There are no orders yet" with "Continue browsing". The status chip counts include cancelled orders while the cards exclude them (chips Completed 17 + Cancelled 13 + New 13 + Processing 6 = 49; YTD card 36). This is an unmerged PR build (vc-frontend#2552 with vc-module-x-frontend#12), not a release.
