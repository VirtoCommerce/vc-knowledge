---
id: KB-AE72A16D
subject: "organizationReturns is granted per organization membership: a role holding xapi:my_organization:return:view on the member's membership in that org opens it; without it the query is refused"
plane: experiential
question: What lets a storefront user see their organization's returns via Query.organizationReturns, and what does a user without it get?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.organizationReturns
  - coordinate: PUT /api/customer/organization-memberships/{id}
evidence:
  - method: observation
    deployment: virtostart
    build: f265d5c82d66eb70
    conditions: module:VirtoCommerce.Return=3.1005.0; store=B2B-store
    at: 2026-10-15T16:33:58.331Z
    by: session:3ec30154
    who: Lenajava1
---
On virtostart (Return 3.1005.0) no role carried xapi:my_organization:return:view. A role holding only that permission was created and ADDED to an Organization maintainer's existing membership (PUT /api/customer/organization-memberships/{id} with roles = existing + new). With a fresh token switched into that organization, organizationReturns(storeId, organizationId) returned every non-Draft return on the org's orders, across buyers (5 for one org: Approved, PartiallyApproved, Processing, Completed, Requested; the Draft was absent). A Purchasing agent in another org without the role got the GraphQL error "The organization's returns are not available to this contact." The built-in Organization maintainer role alone does not grant it on this build.
