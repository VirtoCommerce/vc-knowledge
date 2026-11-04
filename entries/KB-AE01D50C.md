---
id: KB-AE01D50C
subject: requestMfaChangeCode inside the resend interval answers succeeded true with a new twoFactorToken and sends no email, not resend_too_soon; while the user mode is Required it still sends a code
plane: experiential
question: What does Mutation.requestMfaChangeCode answer for a second request within the resend interval, and for a user whose mode is Required?
questions:
  - text: Does requestMfaChangeCode return resend_too_soon?
  - text: Is the change code request refused when the administrator set the user to Required?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.requestMfaChangeCode
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.StoreMode=CustomerChoice
    at: 2026-11-04T12:50:50.286Z
    by: session:325c07df
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    at: 2026-11-04T13:08:28.116Z
    by: session:c4ca0fd7
    bodyHash: 1dffe05110b7
    who: Lenajava1
    note: re-observed on vcst 2026-11-04 by the 110a author proof run, matches
---
CustomerChoice run store, global off. Two requestMfaChangeCode calls back to back: both succeeded true, resendSecondsRemaining 30, errorCode null, a twoFactorToken each; the journal held one MfaSignInEmailNotification after 15 s, and the first ticket with that code still confirmed updateMfaUserMode (succeeded). Separately, with the user mode set to Required by the admin, requestMfaChangeCode answered succeeded true and one email was sent; updateMfaUserMode then answered mfa_locked_by_administrator. Outside CustomerChoice it answers mfa_self_service_not_allowed and sends nothing.
