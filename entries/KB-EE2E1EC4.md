---
id: KB-EE2E1EC4
subject: When GA reads exceed the store's GoogleAnalytics4.DataApi.RequestTimeoutSeconds, the sales-rep activity queries return HTTP 200, no errors[], isAnalyticsAvailable false, 0 for the failed GA categories and real DB counts; GA results are cached per calendar-day range, not per instant
plane: experiential
question: What do salesRepActivities, salesRepCustomerActivitySummary and salesRepCustomerInsights return when the Google Analytics read times out?
questions:
  - text: How do I force a GA-unavailable state for the sales-rep activity page?
  - text: Does a changed period.to timestamp bypass the sales-rep GA cache?
  - text: What does a partial GA failure look like in salesRepActivities categoryCounts?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepActivities
  - coordinate: Query.salesRepCustomerActivitySummary
  - coordinate: Query.salesRepCustomerInsights
  - coordinate: POST /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; module:VirtoCommerce.GoogleEcommerceAnalytics=4.1005.0-pr-61-5267; platform=3.1085.0
    at: 2026-10-19T17:52:40.434Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
    note: "distinct: KB-6880ED54 is about cold reads failing at the ~60 s gateway limit; this is the new configurable deadline setting, the partial-failure response shape per category, the storefront rendering of it, and the per-date-range cache key that decides whether a fault is visible at all."
---
Store B2B-store, setting GoogleAnalytics4.DataApi.RequestTimeoutSeconds set to 1 for ~3 minutes, plain rep token on POST /graphql/sales-rep. A period whose from/to fell on dates already read in the last 60 minutes still answered in ~0.7 s with isAnalyticsAvailable true even though period.to differed by seconds: the GA cache is keyed by the date range, so a new instant on the same day is a cache hit. A period on dates not read before (from 2026-02-03) failed: salesRepActivities returned HTTP 200, no errors[], isAnalyticsAvailable false, orders and customers counts unchanged, searches=0 and productViews=0 while logins still carried 25 (one GA read succeeded, the others hit the deadline) - a partial failure; salesRepCustomerActivitySummary returned createdOn with lastWebLogin/visitsCount/lastSearchTerm/lastViewedProduct null/0 and isAnalyticsAvailable false; salesRepCustomerInsights returned empty or partial lists with isAnalyticsAvailable false. The storefront showed All (-) plus (-) on Searches/Product views/Logins, real Orders/Customers counts and the notice "Some tracked activity couldn't be loaded, so these figures may be incomplete."; the profile Customer activity widget showed Created on plus "Web activity isn't available right now". A dashboard My activity widget whose this-year read was cached kept showing data (isAnalyticsAvailable true).
