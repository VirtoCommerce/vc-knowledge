---
id: KB-C02BCFE3
subject: "An OTP code is used up even when the grant is then refused for a sign-in eligibility reason: after an Invited-membership refusal the same code is invalid_code and counts a failed attempt"
plane: experiential
question: If an otp_email grant is refused after the code verified (e.g. user_invitation_pending_in_organization), can the same code be reused once the cause is fixed?
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: PUT /api/customer/organization-memberships/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; module:VirtoCommerce.Customer=3.1033.0; setting:OtpSignIn.Enabled=true (store); membership:status=Invited (single organization)
    at: 2026-11-18T13:12:53.222Z
    by: session:6da8340c
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; module:VirtoCommerce.Customer=3.1033.0; setting:OtpSignIn.Enabled=true (store); membership:status=Invited (single organization)
    at: 2026-11-18T13:52:58.661Z
    by: session:6da8340c
    bodyHash: 065106c49e1d
    who: Lenajava1
    note: "Re-run with the organization and membership created via POST /api/members + /api/customer/organization-memberships: accessFailedCount read 0 right after the Invited refusal and 1 only after the reuse of the same code (so the eligibility refusal itself is not counted; the reuse is); a fresh code then signed in with organization_id returned 200 + access_token."
---
On vcst (VirtoCommerce.OTP 3.1002.0-pr-4-83d4, Customer 3.1033.0) a disposable buyer with ONE organization membership set to status Invited: a fresh code granted with organization_id of that organization returned 400 code user_invitation_pending_in_organization (no token). The membership was then set back to Inherit; the SAME code returned 400 invalid_grant / invalid_code, and the user's accessFailedCount read 1. So the code is consumed when it is verified, before the organization/eligibility checks; the user needs a new code.
