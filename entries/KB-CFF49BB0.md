---
id: KB-CFF49BB0
subject: "With every organization membership locked, the storefront user sees only their own orders: organizationOrders, organization(id) and org order details are Forbidden, and /account/orders drops the All orders tab"
plane: experiential
question: When all of a B2B contact's organization memberships are locked, do /account/orders and the dashboard still expose the locked organizations' orders?
questions:
  - text: Does a locked org member still see organization orders on the storefront?
  - text: What does Query.organizationOrders return for a contact whose memberships are all locked?
  - text: Can a token requested with organization_id of a locked organization read that organization's data?
  - text: What happens on /company/members when the user has no organization in context?
status: active
appliesTo:
  - axis: membership-state
    value: all-locked
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /account/orders
  - coordinate: Query.organizationOrders
  - coordinate: Query.organization
  - coordinate: Query.orders
  - coordinate: /company/members
  - coordinate: /company/info
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: platform=3.1097.0; module:VirtoCommerce.Customer=3.1033.0; module:VirtoCommerce.Orders=3.1020.0; module:VirtoCommerce.XOrder=3.1017.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1024.0; module:VirtoCommerce.Xapi=3.1031.0; theme=2.62.0-alpha.2647; store=B2B-store; role=org-maintainer+org-employee
    at: 2026-11-12T11:19:12.376Z
    by: session:9c36bcd2
    who: Aleksandra-Mitricheva
    note: "distinct: KB-78188952 covers sign-in and page rendering, not data scope. This entry records whether org-scoped DATA (organization orders, org details, org order by number) is readable in that state. It is not: Forbidden on the server, no All orders tab in the UI, and /company/* redirects."
---
Contact who is org-maintainer in one AGENT-TEST org and org-employee in another, with 0 own orders. Before the lock, xAPI organizationOrders returned 115 and 15 orders for the two orgs, organization(id) returned addresses and contacts, and /account/orders showed All orders / My orders tabs listing the org's orders. After both memberships were permanently locked through the REST lock endpoint, every freshly minted password-grant token (with or without organization_id naming a locked org) had me.contact.organizationId null. organizationOrders and organization(id) for both orgs returned GraphQL errors 'Access denied' with code Forbidden, and order(number) of an org order was Forbidden. orders without userId, with another buyer's userId, and with an organizationId filter all returned totalCount 0. In the storefront, /account/orders rendered without the All/My orders tabs and showed 'There are no orders yet' (GetOrders with the user's own userId returned 0). The dashboard widgets showed 0 orders and an empty My recent orders list, the Corporate sidebar section was absent, /account/quotes was empty, and /company/members and /company/info redirected to /account/dashboard. After unlock, the same reads returned 115/15 again. So the no-org state leaks no organization data: the server denies it and the UI does not request it.
