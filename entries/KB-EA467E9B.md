---
id: KB-EA467E9B
subject: a per-organization sales-rep membership is the served-customer relationship itself
plane: experiential
question: Can a sales rep hold sales-rep:access and serve no customers, and what does salesRepCustomers return then?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCustomers
  - coordinate: /company/my-customers
  - coordinate: /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:18:42.170Z
    by: session:memimpor
    who: Lenajava1
---
A user is a sales rep when they hold sales-rep:access, granted either globally or per organization through an organization membership - and that per-organization membership is also what makes the organization one of the rep's customers, so the two cannot be separated. salesRepCustomers gates on authentication, not the permission: a plain signed-in buyer and a global-only rep both get 200 with totalCount 0, so an empty result does not prove someone is a rep. The storefront /company/my-customers guard resolves the permission in an organization context; a global-only rep has none and is redirected to /account/dashboard, so an empty My customers table is unreachable. The module's schema is served at /graphql/sales-rep.
