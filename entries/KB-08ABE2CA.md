---
id: KB-08ABE2CA
subject: POST /graphql refuses cross-site simple requests with extensions.code CSRF_PROTECTION, and refuses an unauthenticated JSON mutation with Unauthorized
plane: experiential
question: Is the storefront /graphql endpoint protected against CSRF, and what does a cross-origin mutation without a token return?
status: active
appliesTo:
  - axis: flow
    value: csrf
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /graphql
  - coordinate: Mutation.addItem
  - coordinate: Mutation.createOrderFromCart
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; theme=2.59.0
    at: 2026-11-05T14:33:27.834Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
---
On vcptcore_regression (platform 3.1076.8, theme 2.59.0) a script POST to the storefront host /graphql with Origin set to a foreign origin and Content-Type text/plain, application/x-www-form-urlencoded or multipart/form-data answered 400 with errors[0].extensions.code CSRF_PROTECTION (a non-empty GraphQL-Require-Preflight header is required) for addItem and createOrderFromCart. The same JSON body with Content-Type application/json and no Authorization answered 200 with errors[0].extensions.code Unauthorized and data null; the signed-in user's cart was unchanged. The storefront client authenticates every /graphql request with an Authorization Bearer header, not a cookie. No Access-Control-Allow-Origin header was returned for the foreign origin.
