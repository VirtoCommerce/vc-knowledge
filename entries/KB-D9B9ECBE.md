---
id: KB-D9B9ECBE
subject: /graphql and /connect/token return no CORS headers for a foreign origin; only the storefront SPA-shell fallback sends Access-Control-Allow-Origin *
plane: experiential
question: Does the platform or storefront allow cross-origin requests to /graphql, /connect/token and /api?
status: active
appliesTo:
  - axis: flow
    value: cors
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: OPTIONS /graphql
  - coordinate: OPTIONS /connect/token
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; theme=2.59.0
    at: 2026-11-05T14:33:28.740Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
---
On vcptcore_regression, requests with Origin set to a foreign origin: OPTIONS /graphql answered 404 with no CORS headers on both the storefront and the platform host; OPTIONS /connect/token answered 400 invalid_request with no CORS headers; POST /graphql and GET /api/platform/security/currentuser on the platform host carried no Access-Control-Allow-Origin and no Access-Control-Allow-Credentials. Only the storefront host's SPA-shell fallback (text/html, including for /api/... paths that do not exist there) answered Access-Control-Allow-Origin *.
