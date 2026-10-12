---
id: KB-08FAC65C
subject: salesRepCustomers sort my-last-orders ranks by the last NON-cancelled rep order, while the row's lastOrder can be a cancelled order, so a row showing the newest order date can sort last
plane: experiential
question: Does salesRepCustomers sorted by my-last-orders use the same order as the lastOrder field it displays, when the rep's newest order in that organization is cancelled?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCustomers
  - coordinate: POST /graphql/sales-rep
  - coordinate: PUT /api/order/customerOrders
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-12T07:58:06.406Z
    by: session:c1b20b2e
    who: Aleksandra-Mitricheva
---
SalesRep 3.1013.0-pr-18-42c1, five served organizations. The rep placed an order in organization A, its newest, and salesRepCustomers(sort:"my-last-orders:desc") moved A to the top with lastOrder = that order. An admin then cancelled it (PUT /api/order/customerOrders, isCancelled=true, status Cancelled). Afterwards A's row still shows lastOrder = the cancelled order, status Cancelled, created today, which is the newest date of any row. But A sorts LAST under my-last-orders desc, behind organizations whose last orders are weeks older. The position matches A's last non-cancelled order (a timestamp 10 s earlier than the next row's). Reads were fresh (K equals the cold twin with a different `first`) on both replicas, so this is not a stale cache. The sort key excludes cancelled orders (statistics scope, BL-SR-005) but the displayed lastOrder includes them, so the list looks unordered to a reader.
