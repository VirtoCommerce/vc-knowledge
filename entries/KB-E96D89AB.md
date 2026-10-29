---
id: KB-E96D89AB
subject: A token minted on the platform host /connect/token is ignored by the storefront host /graphql (Query.me answers Anonymous), while the platform /graphql honours it
plane: experiential
question: Can a runner call the storefront host /graphql with a Bearer token obtained from the platform host /connect/token?
questions:
  - text: Why does Query.me return userName Anonymous with a valid Bearer on the storefront /graphql?
  - text: Which /graphql host should a test runner use with a password-grant token?
status: active
appliesTo:
  - axis: client
    value: runner
  - axis: host
    value: storefront
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /graphql
  - coordinate: POST /connect/token
  - coordinate: Query.me
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: platform=3.1091.0-pr-3144-9e58; theme=2.61.0; store=B2B-store
    at: 2026-10-29T18:09:29.016Z
    by: session:e80d36e0
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: platform=3.1091.0-pr-3144-9e58; store=B2B-store
    at: 2026-10-29T18:43:48.316Z
    by: session:e80d36e0
    bodyHash: 2d2b187566e8
    who: Lenajava1
    note: "Both directions, measured: platform-minted token on storefront /graphql and storefront-minted token on platform /graphql each read me Anonymous, and user-scoped fields (signInMethods, passkeyCreationOptions) answer HTTP 200 errors[].code Unauthorized. A token works only on the host that minted it. passkeyCreationOptions rp.id follows the request host (platform host -> rp vcst-qa platform host)."
---
On vcst_qa a Node fetch POST {platform host}/connect/token grant_type=password (storeId B2B-store) returned 200 with an access token. The same Bearer on POST {platform host}/graphql { me { id userName passwordExpired } } returned the buyer (passwordExpired true); on POST {storefront host}/graphql it returned HTTP 200 with me.userName "Anonymous" and a random id - the header was not honoured and no error was raised. Tokens the storefront mints itself carry iss = the storefront host (seen in the browser's own requests), so an issuer mismatch is the likely cause (not verified). A case that reads me or another user-scoped query from the runner must use the platform /graphql, or a token minted on the storefront host; otherwise it silently reads the anonymous user.
