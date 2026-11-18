---
id: KB-90C2CF0F
subject: On vcst the otp_email temporary lockout lasts 899 s after 5 failures, OTP and password failures share one counter, and a code issued before the lock is refused invalid_code once the lock expires
plane: experiential
question: How long is the temporary lockout at POST /connect/token after repeated wrong OTP codes, do password and OTP failures share a counter, and does the code still work after the lock expires?
questions:
  - text: Do wrong passwords and wrong OTP codes count toward one lockout?
  - text: What lockoutSecondsRemaining does an OTP lockout return?
  - text: Can a code requested before an OTP lockout be used after the lock expires?
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/otp/request
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; store=B2B-store; setting:OtpSignIn.Enabled=true; user:lockoutEnabled=true
    at: 2026-11-18T15:33:02.585Z
    by: session:6da8340c
    who: Lenajava1
    note: "distinct: KB-C6A3B3B2 is about which codes survive an ACCEPTED code (single-use, resend, parallel grants); this is about WRONG-code lockout: its 899 s duration on vcst, the password+OTP shared counter, and that a never-accepted code has expired by the time the lock ends."
---
REST only, store B2B-store, lockoutEnabled Customer accounts. (1) Five otp_email grants with a wrong code: attempts 1-5 answered 400 invalid_grant/invalid_code (the 5th, on which the lock engages, still invalid_code); attempt 6 and the CORRECT code then answered 400 invalid_grant, code user_is_temporary_locked_out, lockoutSecondsRemaining 899, no token. After waiting 899 s the correct code (requested about 15 minutes earlier, never accepted) answered 400 invalid_grant/invalid_code "The code is invalid or has expired." - the lock outlasts the code, so a new code is needed. Sign-in log: OTP rows InvalidCode and one LockedOut. (2) On another account: 2 wrong-password grants, 2 wrong-code otp_email grants, 1 wrong-password grant (5 failures total) locked it: the correct password and the correct code were both refused, the code with user_is_temporary_locked_out and lockoutSecondsRemaining > 0 - one shared failed-attempt counter.
