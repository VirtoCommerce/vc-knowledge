---
id: KB-661CE0DE
subject: On the PR #30 build, Query.salesRepCoupons returns the store's public promotion coupons to a rep (totalCount 25) and an empty 200 list to a buyer, a rep with no served orgs, and an empty storeId; the profile widget and page both read it
plane: experiential
question: What does Query.salesRepCoupons on /graphql/sales-rep return for a rep, a buyer, a no-customer rep and an empty storeId, and where does the storefront show it?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCoupons
  - coordinate: /company/my-customers/{id}/coupons
  - coordinate: /company/my-customers/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; module:VirtoCommerce.Marketing=3.1010.0; theme=2.62.0-pr-2596-e749-e749b47f; store=B2B-store
    at: 2026-11-03T16:50:06.776Z
    by: session:c3b82fae
    who: Lenajava1
---
Stand with SalesRep 3.1017.0-pr-30-1e10 (open PR #30), password-grant tokens with storeId=B2B-store. salesRepCoupons(storeId:"B2B-store", first:3) returned totalCount 25 for a rep serving 5 orgs and for a rep serving 1 org (same count: the list is the store's coupons, not per customer). The same call returned totalCount 0 (HTTP 200, no errors) for SR_REP_NOCUSTOMERS (rep with no served org) and for an organization buyer, and totalCount 0 for the rep with storeId:"". Anonymous callers are refused like every other scoped query. In the storefront (theme 2.62.0-pr-2596, also an open-PR build) /company/my-customers/{organizationId} shows an 'Available coupons' widget with five rows (name, description, 'Unlimited' or 'N of M uses left', 'Expires <date>', click-to-copy code) and an 'All promotion coupons' link to /company/my-customers/{organizationId}/coupons, whose H1 is 'Promotion coupons for <org>', 10 per page with Previous/Next. The route is declared in plugin.json as my-customers/:organizationId/coupons with when can sales-rep:access. The scoped schema on this build has 30 queries and 6 mutations; salesRepCoupons is the one that exists only because of PR #30.
