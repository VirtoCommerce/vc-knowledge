---
id: KB-E7BBCECF
subject: An impersonation token of an expired-password customer carries no authentication-method claim and the expiry is not enforced on it or on its refresh
plane: experiential
question: Is an expired password enforced on a session opened with grant_type=impersonate?
questions:
  - text: What does me.passwordExpired return in an impersonated session of an expired-password user?
  - text: Does an impersonate-grant token carry the authenticationmethod claim?
status: active
appliesTo:
  - axis: feature
    value: impersonation
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
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.Xapi=3.1029.0
    at: 2026-11-02T08:57:13.297Z
    by: session:44093462
    who: Lenajava1
---
Administrator operator token as Bearer, POST /connect/token grant_type=impersonate user_id=<expired customer> storeId=B2B-store -> 200. The access token has vc_operator_user_id and vc_operator_name but NO authenticationmethod claim. GET /api/platform/security/currentuser with it: authenticationMethod=null, isPasswordSignIn=false, isSsoAuthenticationMethod=true, passwordExpired=false. xAPI me.passwordExpired=false, forcePasswordChange=false, and no operation answered PasswordExpired. A refresh_token grant from it kept the operator claims, still had no method claim and me.passwordExpired stayed false. (The user record's passwordExpired stays true throughout.)
