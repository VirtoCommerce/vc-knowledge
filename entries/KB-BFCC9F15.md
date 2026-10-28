---
id: KB-BFCC9F15
subject: With OtpSignIn.Enabled on, the storefront /sign-in code view still offers 'Sign in with Passkey', and a passkey sign-in completes with no emailed code step
plane: experiential
question: On a store with OTP sign-in enabled, is the passkey button still shown on /sign-in and does a passkey sign-in skip the code step?
questions:
  - text: Does turning OtpSignIn.Enabled on hide the Sign in with Passkey button?
  - text: Does a passkey sign-in on an OTP store ask for an emailed code?
  - text: Which signInType rows does the sign-in log record for OTP and passkey sign-ins?
status: active
appliesTo:
  - axis: feature
    value: otp-email-sign-in
  - axis: feature
    value: passkey-sign-in
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: POST /connect/token
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144; theme=2.61.0-pr-2601-8058; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; module:VirtoCommerce.OTP=3.1001.0; store=B2B-store; setting:OtpSignIn.Enabled=true
    at: 2026-10-28T16:01:52.336Z
    by: session:56fb99bb
    who: Lenajava1
---
2026-10-28 on vcptcore_qa (platform 3.1091.0-pr-3144, theme 2.61.0-pr-2601, VirtoCommerce.Passkeys 3.1000.0-pr-1, VirtoCommerce.OTP 3.1001.0, store B2B-store with OtpSignIn.Enabled=true): /sign-in opens on the code view ("We'll email you a code. No password needed.", Email, Continue, "Sign in with a password instead") and the right-hand column still shows "Sign in with Google" and "Sign in with Passkey"; the passkey button stays on the "Check your email" step too. A buyer signed in by emailed code, registered a key on /account/security, signed out, then clicked "Sign in with Passkey": POST {storefront}/connect/token grant_type=passkey storeId=B2B-store -> 200, the page went straight to / with the account button naming the buyer and no code step. POST /api/platform/security/sign-in-log/search (signInTypes OTP, Passkey) after T0 returned exactly two succeeded rows: signInType OTP then signInType Passkey, storeId B2B-store.
