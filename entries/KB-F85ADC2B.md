---
id: KB-F85ADC2B
subject: A rep's own cart or order change in a served organization makes every org-scoped sales-rep statistic fresh on every instance from the first read after the mutation returns, including cancel, clearCart and removeCart
plane: experiential
question: After a sales rep changes a cart or an order in a served organization, how soon do the cached statistics on /graphql/sales-rep reflect it on every backend instance?
status: active
appliesTo:
  - axis: layer
    value: platform-cache
  - axis: module
    value: sales-rep
  - axis: surface
    value: api
  - axis: version
    value: salesrep 3.1013.0-pr-18
anchors:
  - coordinate: POST /graphql/sales-rep
  - coordinate: Query.salesRepCustomerCartStatistics
  - coordinate: Query.salesRepCustomerOrderStatistics
  - coordinate: Mutation.clearCart
  - coordinate: Mutation.removeCart
  - coordinate: PUT /api/order/customerOrders
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T21:58:55.530Z
    by: session:391bbdbb
    who: yuskithedeveloper
---
SalesRep 3.1013.0-pr-18 (VCST-5755) on vcst_qa, 2026-10-08, statistics TTL 60 min, InvalidateOnChange flags at their defaults (Cart/Order/CustomerCounts true) and TopSeller set true. For a rep serving one organization: each key was warmed with 15-25 cookie-less reads (both instances), then the rep changed data through /graphql (addItem, changeCartItemQuantity, clearCart, removeCart, createOrderFromCart) or an admin cancelled the rep's order (PUT /api/order/customerOrders with isCancelled=true). Reads fired 0 ms after the mutation response (8-10 in parallel) and 20 sequential reads matched a never-read twin key in every case: salesRepCustomerCartStatistics, salesRepCustomerOrderStatistics (period and comparison), salesRepCustomerCounts, salesRepCustomers inline orderStatistics, salesRepOrderFilterRules status vocabulary, salesRepTopSellers, and the all-served keys with organizationId omitted or empty string. A cancelled order drops out of every figure (count and total back to 0) but stays as the row's lastOrder with status Cancelled and keeps 'Cancelled' in the status vocabulary. A read storm DURING a cart mutation (4 loops, ~200 reads) left no stale entry behind. Once in ~14 sequences, 5 of 10 parallel salesRepTopSellers reads finishing 200-270 ms after an order response were stale; all reads from ~700 ms on were fresh: the backplane delay is real but under a second.
