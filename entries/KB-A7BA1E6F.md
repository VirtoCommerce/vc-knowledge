---
id: KB-A7BA1E6F
subject: GET /api/platform/security/currentuser after an otp_email sign-in returns no storeId field, so it cannot show which store the account belongs to
plane: experiential
question: Does GET /api/platform/security/currentuser return the signed-in customer's storeId?
questions:
  - text: Which fields does currentuser return for an OTP-signed-in customer?
  - text: How do I read a customer's home store from its own token?
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/platform/security/currentuser
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; store=B2B-store
    at: 2026-11-18T15:32:53.906Z
    by: session:6da8340c
    who: Lenajava1
---
A Customer account whose home store is a store in B2B-store trustedGroups signed in with grant_type=otp_email on B2B-store (200, access_token). GET /api/platform/security/currentuser with that token answered 200 with only: permissions (empty), userName, isAdministrator=false, passwordExpired, daysTillPasswordExpiry, authenticationMethod=OTP, isSsoAuthenticationMethod=true, isPasswordSignIn=false, memberId, canAccessAdminUI=false, id. There is no storeId; the account's storeId is readable as admin via GET /api/platform/security/users/{userName}.
