---
id: KB-6FAD0B85
subject: On /graphql, a sales rep querying order(id) for another customer's order in an organization the rep neither serves nor belongs to gets errors[] Access denied (Forbidden) with data.order null, while the scoped salesRepCustomerOrder returns plain null
plane: experiential
question: What does xAPI Query.order(id) return to a sales rep for a buyer's order in an organization the rep does not serve?
questions:
  - text: Can a sales rep open another organization's order by id through the storefront xAPI?
  - text: Does Query.orders list orders of an organization the sales rep does not serve?
  - text: Is a rep-authored order in an unserved organization visible to the rep via Query.order?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Query.order
  - coordinate: Query.orders
  - coordinate: Query.organizationOrders
  - coordinate: Query.salesRepCustomerOrder
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    at: 2026-10-20T08:16:48.625Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
---
Seeded an order (status New, B2B-store) whose customerId is a buyer contact who is the only member of an organization; the sales rep caller holds no membership in that organization and is not the order's customer. With the rep's own storefront token (password grant with storeId): Query.order(id) and Query.order(number) on /graphql return HTTP 200, data.order null, errors[0].message "Access denied." with extensions.code Forbidden; Query.orders(filter:<number>) returns totalCount 0; Query.organizationOrders(organizationId:<that org>) returns errors Forbidden; on /graphql/sales-rep salesRepCustomerOrder(id) returns data null with NO errors[]. Control in the same session: Query.order(id) DOES return (no error) both a buyer-placed order in an org the rep serves, and a rep-authored order (customerId = rep) in the unserved org — so Query.order authorizes by customer OR org membership, and only an order that is neither the rep's own nor in a rep's org is refused. A negative test of org scope on Query.order must therefore use an order whose customer is not the rep, and expect errors Forbidden, not a bare null.
