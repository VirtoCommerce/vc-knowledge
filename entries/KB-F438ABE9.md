---
id: KB-F438ABE9
subject: GET /api/platform/security/currentuser answers 200 with an empty user, not 401, for a missing, garbage or signed-out bearer token
plane: experiential
question: Does GET /api/platform/security/currentuser answer 401 when the bearer token is invalid or belongs to a signed-out session?
questions:
  - text: Can currentuser be used to prove a token was revoked after sign-out?
  - text: What does currentuser return without an Authorization header?
  - text: Is a 200 from currentuser proof that a token is valid?
status: active
appliesTo:
  - axis: auth
    value: customer
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/platform/security/currentuser
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144; theme=2.61.0-pr-2601
    at: 2026-10-28T15:04:04.494Z
    by: session:56fb99bb
    who: Lenajava1
---
GET /api/platform/security/currentuser returned HTTP 200 in every case: with a valid Customer password-grant token it named the user (userName set); with no Authorization header, with a garbage bearer value, and with the access token of a storefront session after the user signed out via the account menu, it returned 200 with an empty user object (no userName, no permissions). A status-code check on this endpoint therefore cannot distinguish a valid token from a revoked one; read the body (userName) or use an authorize-required call instead.
