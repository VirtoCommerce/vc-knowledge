---
id: KB-F21AEABC
subject: "/connect/authorize on the platform and storefront hosts accepts plain PKCE and ignores prompt=login: a signed-in session gets a code without re-authentication"
plane: experiential
question: Does /connect/authorize refuse plain PKCE, and does prompt=login force a fresh sign-in?
questions:
  - text: Which PKCE methods does the platform authorization server accept?
  - text: Is prompt=login honoured by the Virto OAuth server?
  - text: Which /connect/authorize parameters are rejected before sign-in?
status: active
appliesTo:
  - axis: platform
    value: 3.1096.0
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /connect/authorize
  - coordinate: POST /connect/token
  - coordinate: GET /.well-known/openid-configuration
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; client=claude-admin-qa (public, PKCE, explicit consent)
    defect: suspected
    at: 2026-11-10T14:20:56.545Z
    by: session:40688c0a
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    at: 2026-11-10T15:15:30.156Z
    by: session:40688c0a
    bodyHash: b79b4e3d57a5
    defect: suspected
    who: Lenajava1
    note: "15:13-15:15Z re-observed: A1 evil redirect 400 ID2043, A2 no challenge 400 ID2054, A4 unlisted resource 400 invalid_target ID2190, A3 plain 302 to login; with an operator session the plain request issued a code with no consent page and /connect/token returned an access token for the plain verifier; prompt=login on the signed-in session issued a code with no login page; consent page X-Frame-Options DENY + frame-ancestors 'none'; /connect/session 404 on the platform host."
---
2026-11-10, public client claude-admin-qa (PKCE required). Discovery advertises code_challenge_methods_supported [plain, S256] and prompt_values_supported incl. login. Without a session: unregistered or other-port redirect_uri -> 400 ID2043 (no redirect); missing code_challenge -> 400 ID2054; resource not registered -> 400 invalid_target ID2190; resource of another client -> 400 ID2192; relative resource -> 400 ID2030; code_challenge_method=plain or a challenge with no method -> 302 to the login page like a valid request. With a platform session, a plain-PKCE request returned a code, /connect/token exchanged it with the plain verifier, and the token (aud {BACK_URL}/admin/mcp) served tools/list. prompt=login with an existing session returned a code straight away on {BACK_URL} and on {FRONT_URL} (storefront sign-in session) - no login page, no login_required error. The consent page (prompt=consent) is served with X-Frame-Options DENY and CSP frame-ancestors 'none'; GET /connect/session is 404 on the platform host and 200 on the storefront host.
