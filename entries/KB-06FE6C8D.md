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
---
On vcptcore_qa1 (VCST-5748), requested one real code via POST /api/otp/request for a real seeded account, retrieved it from the notification journal (511099), then called POST /connect/token grant_type=otp_email with that code TWICE in a row. Both calls returned 200 with a freshly issued access_token — the second call did not report invalid_code. Root cause (from source): the module stores nothing itself; the code is generated/verified via ASP.NET Core Identity's UserManager.GenerateUserTokenAsync/VerifyUserTokenAsync against the built-in "Email" DataProtectorTokenProvider, which is stateless and time-boxed (not single-use) and is not marked consumed anywhere in VirtoCommerce.Otp.Data.Services.OtpService.VerifyCodeAsync after a successful verification. The module's own README says only that a security-stamp change (e.g. sign-out elsewhere) invalidates outstanding codes — a plain successful sign-in does not bump the security stamp, so the same code keeps validating until its token-provider TTL expires. This is an OBSERVED match to the edge-case library's ECL-4.3 pattern ("Same OTP reused multiple times... remains valid indefinitely... Multi-use vulnerability").
