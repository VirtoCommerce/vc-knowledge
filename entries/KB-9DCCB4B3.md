---
id: KB-9DCCB4B3
subject: On a served customer's orders page in the Sales Rep hub, orders the rep created link to /account/orders/{id} while orders placed by others link to /company/my-customers/{orgId}/orders/{orderId}
plane: experiential
question: Where does an order row on /company/my-customers/{orgId}/orders link to?
status: active
appliesTo:
  - axis: actor
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{orgId}/orders
  - coordinate: /company/my-customers/{orgId}/orders/{orderId}
  - coordinate: /account/orders/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: theme=2.60.0-alpha.2579; platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; store=B2B-store; role=Sales Representative
    at: 2026-10-15T13:14:36.426Z
    by: session:6ee2ca49
    who: Lenajava1
  - method: observation
    deployment: local
    conditions: theme=2.60.0; module:VirtoCommerce.SalesRep=3.1015.0; store=B2B-store; role=Sales Representative; state:orderCreator=rep
    at: 2026-10-20T15:59:08.955Z
    by: session:70c763de
    bodyHash: b049b7a23684
    who: yuskithedeveloper
    note: "Only the rep-created half was observed: all five rows of /company/my-customers/{orgId}/orders were orders whose CustomerId is the rep's user id, every row linked to /account/orders/{orderId}, and opening the first one rendered the host order page (H1 'Order #<number>', breadcrumbs Home / Account / Orders, no left account rail). The plugin source (salesRepOrderRoute in StorefrontApp/src/utils.ts) routes isOwn orders to the host OrderDetails route and all others to SalesRepCustomerOrder."
---
On vcst_qa (theme 2.60.0-alpha.2579, hub from the SalesRep plugin), signed in as a rep, /company/my-customers/{orgId}/orders listed all the customer's orders (creator-agnostic). Rows for orders the rep itself created (the same order shown as "My last order" on My customers) are links to /account/orders/{orderId}, which opens the account order page; rows for orders created by someone else link to /company/my-customers/{orgId}/orders/{orderId}, which renders the hub order page (heading "Order #<number>", Created date, status, totals) with no redirect. The "My last order" date on My customers matched the linked order's Created date.
