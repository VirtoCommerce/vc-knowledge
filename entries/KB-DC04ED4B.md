---
id: KB-DC04ED4B
subject: On XOrder 3.1016.0 an OR clause in the orders/organizationOrders filter escapes the buyer's customer/organization scope and returns other customers' orders
plane: experiential
question: Does the storefront xAPI Query.orders filter keyword stay inside the signed-in buyer's customerId/organizationId scope?
questions:
  - text: Can a buyer see other customers' orders through the order list search?
  - text: What does Query.orders return for filter "OR status:\"Processing\""?
  - text: Does Query.organizationOrders filter stay inside the organization?
  - text: Why does filter "OR" return the whole organization's orders in Query.orders?
status: active
appliesTo:
  - axis: role
    value: storefront-buyer
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.orders
  - coordinate: Query.organizationOrders
evidence:
  - method: observation
    deployment: vcst_qa
    build: 9c3a20bc84b64e74
    conditions: module:VirtoCommerce.XOrder=3.1016.0; module:VirtoCommerce.Search=3.1010.0; module:VirtoCommerce.Xapi=3.1027.0; store=B2B-store
    defect: VCST-6536
    at: 2026-10-26T20:17:58.620Z
    by: session:f8aa714b
    who: kutasinaelena
---
Signed in as storefront buyers (password grant, B2B-store) and called Query.orders with filter "", "Processing", "OR" and "OR status:\"Processing\"". A buyer with zero own orders got 0/0/0 and then totalCount 2269 for the OR status clause, every returned order owned by another customer. A buyer in an organization with 6 own orders got 6, 0, 111 (every order of the organization) and 2378. Query.organizationOrders for that organization gave 111, 0, 111, 2378, with the sampled OR-status results all outside the organization. Every response was HTTP 200 with no errors[]. Cause (per the fix PR): X-Order prepends customerId/organizationId to the parsed keyword as text, and the search phrase grammar's OR turns them into alternatives. This should not happen (BL-AUTH-006: a customer must not see other customers' orders).
