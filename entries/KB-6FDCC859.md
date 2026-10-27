---
id: KB-6FDCC859
subject: The Sales Rep customer-orders grid pager stops at page 100 even when totalCount implies more pages
plane: experiential
question: Is the page count on /company/customer-orders capped at 100 pages?
questions:
  - text: Can a sales rep page past page 100 of customer orders?
  - text: Why does the customer orders pager show 100 as the last page when there are more orders?
  - text: Does salesRepCustomerOrders paging reach every order in the storefront grid?
status: active
appliesTo:
  - axis: actor
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/customer-orders
  - coordinate: Query.salesRepCustomerOrders
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; theme=2.61.0-pr-2576-be2c; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; page-size=10
    defect: suspected
    at: 2026-10-27T20:15:06.425Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
---
Signed in as a sales rep serving about 1,270 orders (page size 10), /company/customer-orders showed a pager whose last button is 100. With a start-only date filter the SalesRepCustomerOrders response reported totalCount 1230 (123 pages expected) and with no filter 1273 (128 expected), yet the pager still ended at 100 and Next was disabled on page 100. Orders beyond the first 1,000 in the current sort cannot be reached by paging; changing the sort direction or narrowing the filter is the only way to see them. Whether this is a deliberate storefront page limit or a defect is not established.
