---
id: KB-06FE6C8D
subject: POST /connect/token grant_type=otp_email accepts the SAME code twice, issuing a fresh valid access token both times — the OTP is not invalidated after a successful sign-in (matches ECL-4.3 "same OTP reused multiple times")
plane: experiential
question: Is an OTP email sign-in code single-use, or can the same code be replayed for a second sign-in
questions:
  - text: Can I sign in again with the same one-time code from my email?
  - text: Is the email one-time password single-use, or does it stay valid until it expires?
  - text: Does a successful OTP sign-in invalidate the code, or only a security stamp change?
  - text: Why does replaying the same otp_email grant return a fresh token instead of invalid_code?
  - text: Which mechanism generates and verifies OTP codes, and why is it not consumed after use?
concepts:
  - id: otp
  - id: access-token
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: ticket
    value: vcst-5748
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/otp/request
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-29T15:39:19.011Z
    by: session:p17964
    who: kutasinaelena
    build: fedae735e2f3d08a
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa1
    build: db5e6e6e527dec12
    conditions: platform=3.1087.0-pr-3134-a3d2; module:VirtoCommerce.OTP=3.1000.0; module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; setting:OtpSignIn.Enabled=true
    at: 2026-10-21T17:54:52.990Z
    by: session:5fa7c31a
    bodyHash: f74e4ce859f8
    defect: VCST-6490
    who: Lenajava1
    note: "Re-observed 2026-10-21 on the released OTP 3.1000.0. One code from POST /api/otp/request was granted three times at POST /connect/token grant_type=otp_email: immediately, again immediately, and again 65 s later. All three returned 200 with distinct access tokens (different jti, same sub). Filed as VCST-6490."
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; setting:OtpSignIn.Enabled=true (store)
    at: 2026-11-18T12:32:01.000Z
    by: session:6da8340c
    bodyHash: f74e4ce859f8
    who: Lenajava1
    contradicts: true
    contested: platform 3.1099.0 vs 3.1074.0-pr-3114-5829-vcst-5748-5829199d; virtocommerce.otp 3.1002.0-pr-4-83d4 vs 3.1000.0-pr-1-3e73
    note: "Fixed on VirtoCommerce.OTP 3.1002.0-pr-4-83d4 (platform 3.1099.0): 3/3 loops, one emailed code granted at POST /connect/token grant_type=otp_email answered 200 with a token, the same code again immediately answered 400 error=invalid_grant code=invalid_code (\"The code is invalid or has expired.\"), no token; again 65 s later the same refusal. Each reuse is a failed sign-in (accessFailedCount +1, Sign-in log row OTP / Failed / InvalidCode)."
---
On vcptcore_qa1 (VCST-5748), requested one real code via POST /api/otp/request for a real seeded account, retrieved it from the notification journal (511099), then called POST /connect/token grant_type=otp_email with that code TWICE in a row. Both calls returned 200 with a freshly issued access_token — the second call did not report invalid_code. Root cause (from source): the module stores nothing itself; the code is generated/verified via ASP.NET Core Identity's UserManager.GenerateUserTokenAsync/VerifyUserTokenAsync against the built-in "Email" DataProtectorTokenProvider, which is stateless and time-boxed (not single-use) and is not marked consumed anywhere in VirtoCommerce.Otp.Data.Services.OtpService.VerifyCodeAsync after a successful verification. The module's own README says only that a security-stamp change (e.g. sign-out elsewhere) invalidates outstanding codes — a plain successful sign-in does not bump the security stamp, so the same code keeps validating until its token-provider TTL expires. This is an OBSERVED match to the edge-case library's ECL-4.3 pattern ("Same OTP reused multiple times... remains valid indefinitely... Multi-use vulnerability").
