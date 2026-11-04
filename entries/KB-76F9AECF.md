---
id: KB-76F9AECF
subject: The email for a requestMfaChangeCode code is the sign-in email MfaSignInEmailNotification ("Your sign-in code"), identical to a sign-in code email and with no purpose field
plane: experiential
question: What email does Mutation.requestMfaChangeCode send, and does it differ from the sign-in code email?
questions:
  - text: Does the MFA change code email say it confirms turning two-factor on or off?
  - text: Which notification type and template does requestMfaChangeCode use?
  - text: Can the MfaSignInEmailNotification template tell a change code from a sign-in code?
  - text: What subject does the two-factor mode change confirmation email have?
status: active
appliesTo:
  - axis: module
    value: mfa
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.requestMfaChangeCode
  - coordinate: POST /api/notifications/journal
  - coordinate: GET /api/notifications/journal/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; module:VirtoCommerce.Notifications=3.1018.0; setting:MultiFactorAuthentication.StoreMode=CustomerChoice; setting:MultiFactorAuthentication.Enabled=false
    at: 2026-11-04T18:33:20.854Z
    by: session:7f21950d
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.StoreMode=CustomerChoice
    at: 2026-11-04T19:48:13.730Z
    by: session:2f6b8998
    bodyHash: da6674f12f88
    who: Lenajava1
    note: "Independent judge session: a requestMfaChangeCode row and a password-grant sign-in row both read subject 'Your sign-in code' and the same body ('If you did not try to sign in, change your password.') apart from the code."
---
CustomerChoice run store, one confirmed customer, global MFA off. Three codes on one account, each requested after the 30 s resend interval: requestMfaChangeCode (its code then turned the user mode on through updateMfaUserMode), a password-grant sign-in challenge (its code completed grant_type=mfa), and requestMfaChangeCode again (its code turned the mode off). The journal (GET /api/notifications/journal/{id}) has three rows with the same notificationType MfaSignInEmailNotification, the same subject "Your sign-in code" and the same body apart from the code: "Your sign-in code is <code>. It expires shortly, so use it right away. If you did not try to sign in, change your password." Same languageCode. No journal field names the purpose. The notification type (GET /api/notifications/MfaSignInEmailNotification for the store tenant) has one predefined template, and its only custom field is code. InputRequestMfaChangeCodeType takes only storeId. In source (vc-module-mfa 1a0f481), the change purpose is kept on the two-factor ticket and read only when the code is verified. It is not passed to IMfaProvider.SendCodeAsync(store, user, code), so the template cannot branch on it. README L113 names this one notification as the sender of the code.
