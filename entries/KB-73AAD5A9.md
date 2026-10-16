---
id: KB-73AAD5A9
subject: Leaving and re-entering /company/my-customers inside the account area refetches the SalesRepCustomers list with identical variables but does not refetch the SalesRepCustomersCount badge query
plane: experiential
question: When a sales rep navigates from /company/dashboard back to /company/my-customers by the left-rail link, which sales-rep queries fire again?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers
  - coordinate: Query.salesRepCustomers
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; module:VirtoCommerce.SalesRep=3.1014.0-pr-13
    at: 2026-10-14T18:07:50.587Z
    by: session:6630b76f
    who: Lenajava1
---
Signed in as a sales rep serving 5 orgs, loaded /company/my-customers (SalesRepCustomersCount + SalesRepCustomers fired), then client-side navigated to /company/dashboard via the Sales Rep hub rail and back via the rail 'My customers' link. On re-entry a fresh SalesRepCustomers request fired whose variables and query text were byte-identical to the first mount. SalesRepCustomersCount did not fire again during the round trip (the dashboard round sent SalesRepLayout, SalesRepCustomerOrderStatistics, SalesRepCustomerCartStatistics, SalesRepCustomerCounts, SalesRep order/top-seller/task queries only). The rail badge is rendered by the persistent account layout, so it refreshes only when that layout mounts (full load), not on in-hub route changes. Badge (5) and rendered rows (5) still agreed because nothing changed in between.
