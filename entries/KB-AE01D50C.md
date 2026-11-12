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
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.StoreMode=CustomerChoice
    at: 2026-11-04T14:14:22.002Z
    by: session:7f21950d
    bodyHash: 1dffe05110b7
    who: Lenajava1
    note: "Re-observed 2026-11-04 with INGRESSCOOKIE pinned: two requests 4 s apart, both succeeded (resend 30 then 26), different tickets, one journal row; the code confirmed ticket 2, then ticket 1 with the same code answered invalid_code. Required user: request succeeded, 1 journal row, update mfa_locked_by_administrator."
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; theme=2.62.0-pr-2622-caac-caac275a; setting:MultiFactorAuthentication.StoreMode=CustomerChoice
    at: 2026-11-04T14:25:04.267Z
    by: session:7f21950d
    bodyHash: 1dffe05110b7
    who: Lenajava1
    note: "Storefront UI, 2026-11-04: Turn on → Cancel → Turn on again 20 s later: second requestMfaChangeCode succeeded with a new ticket and resendSecondsRemaining 10 (the time left from the first), no second journal row; the widget re-shows \"enter the code we sent to <masked>\" with \"Resend code in 8 s\" (it does not claim a new code). Resend after the countdown sent a new email and showed \"New code sent.\". Separately, a change code WAS emailed 24 s after a sign-in code for the same user (org Turn off right after a code sign-in), so the 30 s window is not shared between sign-in and change codes."
  - method: observation
    deployment: local
    build: 34214ec43feee00d
    conditions: module:VirtoCommerce.MFA=3.1003.0-pr-6-2da2; theme=2.62.0-pr-2648-f69c-f69c2e9c; setting:MultiFactorAuthentication.StoreMode=CustomerChoice; setting:MultiFactorAuthentication.Enabled=false
    at: 2026-11-12T10:59:35.681Z
    by: session:af366868
    bodyHash: 1dffe05110b7
    who: Lenajava1
    contradicts: true
    contested: virtocommerce.news 3.1006.0 vs 3.1005.0; virtocommerce.profileexperienceapimodule 3.1024.0 vs 3.1023.0; virtocommerce.xapi 3.1031.0 vs 3.1029.0; virtocommerce.xcart 3.1046.0 vs 3.1043.0-pr-151-07b3; virtocommerce.xcatalog 3.1024.0 vs 3.1024.0-pr-115-8339; …
    note: "Only the Required clause no longer holds on the fixed build (VCST-6688): with the user mode set to Required by the admin (PUT /api/mfa/modes/users/{id}), requestMfaChangeCode with command {storeId} only answered succeeded=false, errorCode mfa_locked_by_administrator, no twoFactorToken, and added no MfaSignInEmailNotification journal row. With command {storeId, organizationId} for the org MFA permission holder (org mode Off) it succeeded and sent one code. The resend-interval clause was not re-tested. The old \"still sends a code\" behaviour was the defect VCST-6688, fixed in VirtoCommerce.MFA 3.1003.0-pr-6-2da2."
---
CustomerChoice run store, global off. Two requestMfaChangeCode calls back to back: both succeeded true, resendSecondsRemaining 30, errorCode null, a twoFactorToken each; the journal held one MfaSignInEmailNotification after 15 s, and the first ticket with that code still confirmed updateMfaUserMode (succeeded). Separately, with the user mode set to Required by the admin, requestMfaChangeCode answered succeeded true and one email was sent; updateMfaUserMode then answered mfa_locked_by_administrator. Outside CustomerChoice it answers mfa_self_service_not_allowed and sends nothing.
