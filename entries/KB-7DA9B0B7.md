---
id: KB-7DA9B0B7
subject: platform REST APIs reject a store- or organization-scoped token with 401
plane: experiential
question: Why does /api/carts/{id} return 401 invalid_token with a token that works on /graphql?
questions:
  - text: Why does a platform REST cart call answer 401 invalid_token with a bearer token that storefront GraphQL happily accepts?
  - text: As QA, do I need a separate admin token when my test mixes storefront GraphQL calls with platform REST writes?
  - text: Is a 401 from the platform API with an organization-scoped token a missing permission or a token audience mismatch?
  - text: Which kind of access token can update cart line quantities through the platform REST endpoints?
  - text: Does requesting a token with a store id or organization id restrict it to the storefront API only?
concepts:
  - id: access-token
  - id: rest-api
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
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
