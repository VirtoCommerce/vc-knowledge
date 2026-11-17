---
id: KB-22E8ECE1
subject: "A product subscription keeps the organization it was bought in: after switching to another organization, the storefront still lists it, skip works, and Order now creates the order under the original organization"
plane: experiential
question: Which organization does a product subscription renewal order get when the buyer has switched to another organization?
questions:
  - text: Are My subscriptions filtered by the current organization?
  - text: Does Order now on a subscription use the current organization context?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/subscriptions
  - coordinate: Mutation.skipProductSubscription
  - coordinate: Query.order
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 9dfbb57eacb0bf2c
    conditions: theme=2.63.0-pr-2602-57ad; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-79cf
    at: 2026-11-17T10:46:53.627Z
    by: session:d9b8e380
    who: kutasinaelena
---
Contact with two organizations, B2B-store. Bought a subscribed line while organization A was selected (initial order organizationId = A). Switched to organization B in the header. /account/subscriptions still listed the organization A subscriptions (the list is per user, with no organization shown), while /account/orders under B listed only B orders. Skip next delivery worked; Order now created a renewal order whose xAPI order.organizationId/organizationName = A and customerId = the user, not B.
