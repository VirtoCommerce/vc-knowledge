---
id: KB-B6E151FC
subject: SaveSalesRepLayout for scope customerProfile sends {scope, storeId, schemaVersion:1, regions} with no organizationId, and the coupons block in mainLeft carries a maxRows setting (default 5) like orders and top_sellers
plane: experiential
question: What does the customer-profile Save layout send in SaveSalesRepLayout, and which blocks carry a maxRows setting?
questions:
  - text: Does the customerProfile layout save carry an organizationId?
  - text: Is the Available coupons widget configurable with Max rows on the customer profile?
  - text: What regions and blocks does SaveSalesRepLayout emit for scope customerProfile?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Mutation.saveSalesRepLayout
  - coordinate: /company/my-customers/{id}
  - coordinate: Query.salesRepCoupons
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: store=B2B-store
    at: 2026-11-03T17:53:59.025Z
    by: session:05e86aa3
    who: Lenajava1
---
Signed in as a sales rep on /company/my-customers/{orgId}, Edit layout, dragged Top sellers above My recent orders, Save layout. The POST /graphql SaveSalesRepLayout command was {scope:"customerProfile", storeId:"B2B-store", schemaVersion:1, regions:[statistics(new_orders, active_cart, mtd, orders_ytd, aov), mainLeft(top_sellers, orders, coupons), mainRight(actions, info, customer_activity)]} with id===type on every block. No organizationId or other customer-scoping field; the saved order then rendered on a different served customer's profile (scope-wide). Settings: orders, top_sellers AND coupons each carry [{key:"maxRows", value:5}]; every other block []. In edit mode the three list widgets show a Max rows spinbutton (My recent orders 1-20, Top sellers 1-10, Available coupons 1-20); stat cards show none. Setting coupons Max rows to 3 makes the widget send SalesRepCoupons first:3 and render 3 cards; hiding the coupons block stops the SalesRepCoupons request entirely. Reset + Save restores the defaults.
