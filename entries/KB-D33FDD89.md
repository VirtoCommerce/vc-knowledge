---
id: KB-D33FDD89
subject: POST /connect/session refuses a password-sign-in token of an expired user and an impersonation token, and creates the cookie session for an OTP token of the same expired user
plane: experiential
question: When does POST /connect/session (bearer token to platform cookie session) refuse an expired-password user?
questions:
  - text: How do I call /connect/session with a bearer token — antiforgery header and returnUrl?
  - text: Does /connect/session accept an impersonation token?
status: active
appliesTo:
  - axis: feature
    value: password-expiry
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /connect/session
  - coordinate: GET /connect/session
evidence:
  - method: observation
    deployment: vcst_qa
    build: 0940c86d8a29dd8a
    conditions: platform=3.1093.0-pr-3154-4607
    at: 2026-11-02T08:57:13.360Z
    by: session:44093462
    who: Lenajava1
---
Recipe: GET /connect/session WITH the same Bearer header returns {requestToken} and an antiforgery cookie; then POST /connect/session?returnUrl=/connect/authorize?... with Bearer, Origin = platform origin, the cookie and header RequestVerificationToken (X-XSRF-TOKEN is not accepted: 400; a request token minted anonymously and posted with a Bearer: 400). Results: no bearer 401; non-expired admin or customer password-grant token 204 + .VirtoCommerce.Identity.Application cookie; expired customer or back-office password-grant token 401; expired customer impersonation token 401 (impersonated principals are refused by design in GetStorefrontSessionUserAsync, independent of expiry); expired customer otp_email token 204 + cookie.
