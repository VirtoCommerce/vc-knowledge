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
  - method: observation
    deployment: vcst_qa
    build: 5bd367d41cd309ee
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.MFA=3.1000.0-pr-1-e8c7; theme=2.61.0-pr-2570-8619; store=B2B-store; setting:MultiFactorAuthentication.StoreMode=Enabled
    at: 2026-10-23T19:24:36.196Z
    by: session:7ed484a4
    bodyHash: ca21b485c920
    who: kutasinaelena
    note: "Unchanged on the newer build: password at 19:10:18Z emailed one code; Back to sign in + Sign in at ~19:10:48Z answered two_factor_required with resendSecondsRemaining=1, no new journal row, UI still \"We sent a code to a***d@example.com.\" with Resend enabled."
  - method: observation
    deployment: vcst_qa
    build: 8d4bda9222c60036
    conditions: platform=3.1090.0-pr-3133; setting:MultiFactorAuthentication.StoreMode=Enabled (disposable store)
    at: 2026-10-26T09:59:06.423Z
    by: session:7ed484a4
    bodyHash: ca21b485c920
    who: kutasinaelena
    note: "Same via raw REST (no storefront): two password grants 0.7 s apart at 09:53:03Z on a disposable MFA-Enabled store both answered two_factor_required with a ticket; the journal got only one new MfaSignInEmailNotification. A grant at 09:54:45Z (past the cooldown) emailed a fresh code."
---
On vcst_qa 2026-10-23 (B2B-store StoreMode=Enabled): password step at 12:38:07Z emailed one MfaSignInEmailNotification. "Back to sign in" then Sign in again at 12:38:26Z: POST /connect/token answered 400 two_factor_required with a new twoFactorToken and resendSecondsRemaining=11, the journal still held ONE notification, but the code step said "We sent a code to a•••4@example.com." with "Resend code in 11 s". Typing the code from the first email on the second ticket answered 200 and signed in — the code is per user, not per ticket.
