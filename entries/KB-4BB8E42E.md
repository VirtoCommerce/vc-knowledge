---
id: KB-4BB8E42E
subject: A second storefront password step within the 30 s MFA resend cooldown sends no new email, yet the code step still says "We sent a code"; the code from the earlier email signs in on the newer ticket
plane: experiential
question: What happens when a buyer repeats the storefront password step within 30 seconds while MFA is required — is a new code emailed, and which code works?
questions:
  - text: Is the MFA code bound to the two-factor ticket or to the user?
  - text: Does going Back to sign in and re-entering the password resend the MFA email?
status: active
appliesTo:
  - axis: feature
    value: mfa
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: d70745c02c9df3fe
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.MFA=3.1000.0-pr-1-cf83; theme=2.61.0-pr-2570-2fc9; store=B2B-store; setting:MultiFactorAuthentication.StoreMode=Enabled
    defect: suspected
    at: 2026-10-23T12:52:05.155Z
    by: session:7ed484a4
    who: kutasinaelena
---
On vcst_qa 2026-10-23 (B2B-store StoreMode=Enabled): password step at 12:38:07Z emailed one MfaSignInEmailNotification. "Back to sign in" then Sign in again at 12:38:26Z: POST /connect/token answered 400 two_factor_required with a new twoFactorToken and resendSecondsRemaining=11, the journal still held ONE notification, but the code step said "We sent a code to a•••4@example.com." with "Resend code in 11 s". Typing the code from the first email on the second ticket answered 200 and signed in — the code is per user, not per ticket.
