---
id: KB-7DA9B0B7
subject: platform REST APIs reject a store- or organization-scoped token with 401
plane: experiential
question: Why does /api/carts/{id} return 401 invalid_token with a token that works on /graphql?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: /api/carts/{id}
  - coordinate: POST /connect/token
  - coordinate: /graphql
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:31.344Z
    by: session:memimpor
    who: Lenajava1
---
A token requested with storeId and/or organization_id is storefront-scoped: storefront GraphQL accepts it, but platform REST endpoints such as /api/carts/{id} reject it with 401 invalid_token - an audience problem, not a missing permission (which would be 403). An administrator token requested with no store and no organization context works: GET /api/carts returns 200 and a line-item quantity PUT returns 204. The same behaviour was seen on two deployments. A flow mixing storefront GraphQL and platform REST writes therefore needs two tokens.
