---
id: KB-ADE54B03
subject: The account Dashboard shows a full-width Sales Statistics block right after the four order stat widgets
plane: experiential
question: What does the Sales Statistics block on /account/dashboard show and where does it sit?
questions:
  - text: Where is the Sales Statistics block on the buyer dashboard?
  - text: Which periods and KPI tiles does the account dashboard sales report offer?
  - text: Does the buyer dashboard chart exclude cancelled orders?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/dashboard
  - coordinate: Query.orderStatistics
evidence:
  - method: observation
    deployment: vcst_qa
    build: 957a892645884c54
    conditions: platform=3.1099.0; theme=2.63.0-pr-2667-0444-0444048a; role=Organization buyer
    at: 2026-11-20T10:47:42.649Z
    by: session:74ee3083
    who: Lenajava1
    note: "distinct: KB-46E8B8DE describes the four stat cards and My recent orders; this entry describes the separate Sales Statistics block (VCST-6258) between them — its periods, KPI tiles and charts."
---
Signed in as an organization buyer on vcst, /account/dashboard shows the four stat cards (Orders placed · Week, · MTD, · YTD, Avg order value), then a full-width "SALES STATISTICS" card with an "All orders →" link, then My recent orders. The block has a period switch (6 months | 12 months | Current year | Q1–Q4; Current year by default), four KPI tiles (Sales, Orders, Average order value, "<Month> to date · day N of M" with a progress bar and a projected month end), and two charts — "Monthly sales · <period>" and "Average order value" — each with its own Last year switch, an "Upcoming" placeholder for future months and a "Cancelled orders excluded" note. Picking Q4 changed the tiles and limited the chart to Oct–Dec. The block calls GetMonthlyOrderStatistics per selected period. The Last year switches expose no pressed or checked state in the accessibility tree (two same-named buttons).
