---
id: KB-F7BA392B
subject: "GET /api/platform/settings/modules/VirtoCommerce.SalesRep returns 9 module settings: SalesRep.Enabled plus four Statistics cache-expiration minutes (default 5) and four InvalidateOnChange flags"
plane: experiential
question: Where are the Sales Rep Statistics cache-expiration settings stored and what are the module's settings and defaults?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: role
    value: administrator
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: GET /api/platform/settings/modules/VirtoCommerce.SalesRep
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10
    at: 2026-11-03T17:02:17.694Z
    by: session:c3b82fae
    who: Lenajava1
---
Read as administrator with GET /api/platform/settings/modules/VirtoCommerce.SalesRep: nine settings. Group 'Sales Rep|General': SalesRep.Enabled (module value null, default true). Group 'Sales Rep|Statistics': SalesRep.Statistics.OrderCacheExpirationMinutes, CartCacheExpirationMinutes, CustomerCountsCacheExpirationMinutes, TopSellerCacheExpirationMinutes (default 5 each; this stand had all four set to 1) and CartInvalidateOnChange (true), CustomerCountsInvalidateOnChange (null, default true), OrderInvalidateOnChange (null, default true), TopSellerInvalidateOnChange (false, default false). So the cache settings live at module scope, not in the store Settings blade. Same read pattern on VirtoCommerce.GoogleEcommerceAnalytics gives GoogleAnalytics4.DataApi.CacheTtlMinutes (default 60), RequestTimeoutSeconds (default 45), PropertyId, and on VirtoCommerce.TaskManagement TaskTypes (default 'Registration Review', value null) and TaskNotificationsEnabled (default false).
