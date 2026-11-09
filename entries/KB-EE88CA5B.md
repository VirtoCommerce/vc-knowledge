---
id: KB-EE88CA5B
subject: An organization member's /account/orders showed "There are no orders yet" for over 5 minutes after a successful checkout, while the order existed in the platform with that organization
plane: experiential
question: Does /account/orders list an organization member's order right after checkout?
questions:
  - text: Why is My orders empty right after placing an order?
  - text: How long until a new order appears on /account/orders for an org member?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders
  - coordinate: POST /api/order/customerOrders/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; theme=2.59.0
    at: 2026-11-09T16:15:45.182Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
A freshly seeded org member (AGENT-TEST-Org-TechFlow organization) placed order CO261109-00022 through storefront checkout on B2B-store; REST POST /api/order/customerOrders/search returned the order (status New, customer = that member, organization set). For more than 5 minutes afterwards /account/orders for the same signed-in member rendered "There are no orders yet". Not established: whether this is order search-index lag or an organization-scoped visibility rule; the list was not re-checked later.
