---
id: KB-27D7DCE3
subject: On a served customer's activity page, salesRepActivities with take 20 / skip 0 returned only 19 items while totalCount was 51, so page 1 of the All list showed 19 rows and page 2 showed 20
plane: experiential
question: Does the first page of the sales-rep customer activity list always hold 20 rows when more rows exist?
questions:
  - text: Why does page 1 of /company/my-customers/{id}/activities show 19 rows?
  - text: Is salesRepActivities paging short by one item?
  - text: How many items does Query.salesRepActivities return for take 20 skip 0?
status: active
appliesTo:
  - axis: feature
    value: customer-activity
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}/activities
  - coordinate: Query.salesRepActivities
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: platform=3.1085.0; theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; module:VirtoCommerce.GoogleEcommerceAnalytics=4.1005.0-pr-61-5267; role=Sales Representative
    defect: suspected
    at: 2026-10-20T11:07:05.345Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
---
A plain rep opened /company/my-customers/{id}/activities for a served organization, chose All time and the All tab (3 pages). The page listed 19 rows on page 1 and 20 on page 2; the SalesRepActivities request behind page 1 (take 20, skip 0, no period, withCategoryCounts false) answered HTTP 200, no errors[], isAnalyticsAvailable true, totalCount 51 and an items array of 19 (3 login buckets, 1 search, 6 orders, 7 login buckets, 2 product views). No row repeated across pages. So the server returns a short first page; whichever filter or merge removes the missing item runs after paging.
