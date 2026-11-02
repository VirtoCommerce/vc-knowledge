---
id: KB-A7BECD3E
subject: A password-grant access token carries the authenticationmethod claim "Password", a refresh keeps it, and an expired password is then enforced on currentuser and xAPI
plane: experiential
question: Which authentication-method claim does a password-grant token carry, and is an expired password enforced on it and on its refreshed token?
questions:
  - text: Does a refresh_token grant keep the authentication method of the original password sign-in?
  - text: What does GET /api/platform/security/currentuser return for isPasswordSignIn with an expired password?
  - text: Does grant_type=mfa stamp the Password authentication method?
  - text: Does Basic authentication count as a password sign-in for password expiry?
status: active
appliesTo:
  - axis: feature
    value: password-expiry
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /connect/token
  - coordinate: GET /api/platform/security/currentuser
  - coordinate: Query.me
evidence:
  - method: observation
    deployment: vcst_qa
    build: 0940c86d8a29dd8a
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.MFA=3.1001.0-pr-4-b211; module:VirtoCommerce.Xapi=3.1029.0; setting:MultiFactorAuthentication.Enabled=false
    at: 2026-11-02T08:57:13.113Z
    by: session:44093462
    who: Lenajava1
---
On an account whose password was expired by an admin reset with forcePasswordChangeOnNextSignIn=true: POST /connect/token grant_type=password (with or without storeId) answered 200 with tokens; the access token (a plain JWT) carries claim http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod = "Password". With that token GET /api/platform/security/currentuser returned isPasswordSignIn=true, isSsoAuthenticationMethod=false, authenticationMethod="Password", passwordExpired=true; xAPI me.passwordExpired=true and forcePasswordChange=true, and gated operations (organizations, cart) answered errors[].code PasswordExpired inside HTTP 200. grant_type=refresh_token from that session returned a token with the same claim "Password" and the same enforcement. A per-user Identity two-factor sign-in (password -> 400 two_factor_required -> grant_type=mfa with the journal code) also produced claim "Password" plus amr=mfa, and its refresh kept both; expiry enforced the same way. Basic authentication with the expired user's credentials on REST resolved the user (currentuser isPasswordSignIn=true, passwordExpired=true); a wrong password answered 401; Basic is not accepted by /graphql (me resolves Anonymous).
