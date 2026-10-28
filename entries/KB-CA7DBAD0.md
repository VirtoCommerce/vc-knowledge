---
id: KB-CA7DBAD0
subject: A successful passkey sign-in does not reset accessFailedCount, while a successful password sign-in does
plane: experiential
question: Does a successful passkey sign-in reset the failed-attempt counter (accessFailedCount)?
questions:
  - text: Can a passkey sign-in sign in a temporarily locked or admin-locked account?
  - text: After 4 wrong passwords and a passkey success, does one more wrong password lock the account?
  - text: What does POST /connect/token grant_type=passkey return for a locked-out account?
status: active
appliesTo:
  - axis: grant
    value: passkey
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/platform/security/users/{id}/lock
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144; module:VirtoCommerce.Passkeys=3.1000.0-pr-1; store=B2B-store
    defect: suspected
    at: 2026-10-28T13:18:29.748Z
    by: session:56fb99bb
    who: Lenajava1
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144-e637; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; store=B2B-store
    at: 2026-10-28T14:01:10.965Z
    by: session:56fb99bb
    bodyHash: 57e36aeedca8
    defect: suspected
    who: Lenajava1
    note: "Re-observed 2026-10-28 13:56: 4 wrong password grants -> accessFailedCount 4; storefront 'Sign in with Passkey' click -> /connect/token passkey 200 (log Passkey ok, store B2B-store); count still 4; one wrong password typed in the storefront form -> 'Your account has been temporarily locked' (log Password LockedOut). Control: 4 wrong -> password success -> count 0 -> 1 wrong -> login_failed, count 1. Temporary lock (5 wrong) -> passkey grant 400 user_is_temporary_locked_out; admin lock -> passkey grant 400 user_is_locked_out; both logged Passkey/LockedOut with user+store."
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144-e637; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; store=B2B-store; lane=playwright-edge
    at: 2026-10-28T14:51:30.828Z
    by: session:56fb99bb
    bodyHash: 57e36aeedca8
    defect: suspected
    who: Lenajava1
    note: "Third live repro 2026-10-28 14:47-14:49 UTC on Edge: 4 wrong password grants -> accessFailedCount 4; one real click on Sign in with Passkey -> one /connect/token passkey 200; count still 4; one wrong password in the storefront form -> 400 user_is_temporary_locked_out. Control re-run: 4 wrong -> password success -> 0 -> 1 wrong -> 1. Source (vc-module-passkeys a25fea03): PasskeyService.AuthenticateAsync never calls ResetAccessFailedCountAsync; GrantTypeHandlerBase (vc-platform e6371864) has no reset either; the sibling OTP grant resets in OtpService on success."
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144-e637; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; store=B2B-store; lane=playwright-edge
    at: 2026-10-28T15:44:07.751Z
    by: session:56fb99bb
    bodyHash: 57e36aeedca8
    defect: suspected
    who: Lenajava1
    note: "Suite 115 PKA-003/PKA-012 15:17-15:20Z: temp lock -> passkey 400 user_is_temporary_locked_out, admin lock -> user_is_locked_out (log LockedOut + user + store); 4 wrong -> passkey 200 -> count stays 4 -> 1 wrong locks, right password then refused."
---
On a storefront B2B buyer with lockout enabled (lock after 5 failures): (1) temporary lock (5 wrong password grants) -> grant_type=passkey on POST /connect/token returns 400 invalid_grant code user_is_temporary_locked_out (storefront shows 'Your account has been temporarily locked. Please try again later.'); (2) admin lock via POST /api/platform/security/users/{id}/lock -> passkey grant returns 400 code user_is_locked_out; both logged in the sign-in log as Type Passkey, failureReason LockedOut, with user and store. (3) Counter: unlock, 4 wrong passwords (accessFailedCount=4), a SUCCESSFUL passkey sign-in left accessFailedCount at 4, and the next single wrong password locked the account (user_is_temporary_locked_out). Control on the same account: 4 wrong passwords then a successful password grant reset accessFailedCount to 0, and one more wrong password gave login_failed with count 1. So the passkey success path skips the reset that the password path performs.
