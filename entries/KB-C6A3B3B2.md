---
id: KB-C6A3B3B2
subject: After an otp_email grant succeeds, that code and every code emailed before it are refused (400 invalid_grant / invalid_code); a code requested later than the accepted one is also refused, while a password sign-in invalidates nothing and two parallel grants with one code yield exactly one token
plane: experiential
question: Which OTP codes still work at POST /connect/token grant_type=otp_email after one code has been accepted, a resend, a password sign-in, or a parallel grant?
questions:
  - text: Is the OTP email code single-use after the VCST-6490 fix
  - text: Does resending an OTP code invalidate the earlier one
  - text: Does a password sign-in invalidate an outstanding OTP code
  - text: Can two concurrent otp_email grants with one code both succeed
  - text: What does the token endpoint return for a reused OTP code
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/otp/request
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; setting:OtpSignIn.Enabled=true (store); user:lockoutEnabled=true
    at: 2026-11-18T12:32:19.003Z
    by: session:6da8340c
    who: Lenajava1
  - method: source
    deployment: vc-module-otp@02f2e2de
    at: 2026-11-18T12:51:55.121Z
    by: session:6da8340c
    bodyHash: b4ca994bef6f
    who: Lenajava1
    note: "Source read at vc-module-otp merge of PR #4: stamp rotated on accept (OtpService.cs:99,176-189), stamp part of token modifier (OtpEmailTokenProvider.cs:10-16), request does not touch stamp (OtpService.cs:52-55), failed verify calls AccessFailedAsync (:90), InvalidCode maps to invalid_grant/invalid_code + sign-in-log reason InvalidCode (OtpGrantTypeHandler.cs:105,125). Not observed live by this session."
---
On vcst (platform 3.1099.0, VirtoCommerce.OTP 3.1002.0-pr-4-83d4, store OTP sign-in on), REST only. (1) One code granted at POST /connect/token grant_type=otp_email: 200 with access_token; the same code again immediately and 65 s later: HTTP 400, body error=invalid_grant, code=invalid_code, errorDescription "The code is invalid or has expired.", errors[0].code=invalid_code, accessToken null - 3/3 loops. (2) Codes A then B requested in different 3-minute windows (distinct codes): grant B 200, then A 400 invalid_code. (3) Same setup: grant A 200 (the resend did not invalidate A), then B 400 invalid_code - a code issued before the accepted one, even if emailed later than it, no longer works. (4) Request code C, sign in with grant_type=password (200), then grant C: 200 - a password sign-in does not invalidate a code. (5) Two concurrent grants with one code: exactly one 200, the other 400 invalid_code, no 5xx. Every refusal raised the user's accessFailedCount by 1 and wrote a Sign-in log row type OTP, outcome Failed, reason InvalidCode; a successful grant reset the count to 0 and wrote an OTP Success row. The refusal body mixes OAuth snake_case (error) with camelCase fields (accessToken, errorDescription, errors[]), while the success body uses access_token.
