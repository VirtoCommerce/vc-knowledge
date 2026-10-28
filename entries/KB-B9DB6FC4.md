---
id: KB-B9DB6FC4
subject: On a store with MultiFactorAuthentication.StoreMode=Enabled, a storefront passkey sign-in (grant_type=passkey) completes with no emailed code step and sends no MfaSignInEmailNotification, while a password sign-in on the same store is challenged
plane: experiential
question: Does a storefront passkey sign-in on a store that requires MFA by email still ask for an emailed one-time code?
questions:
  - text: Is a passkey treated as satisfying store MFA on the storefront?
  - text: Does grant_type=passkey answer two_factor_required when the store's StoreMode is Enabled?
  - text: Is an MFA sign-in email sent after a passkey sign-in on an MFA store?
  - text: Which sign-in log rows does a passkey sign-in write on an MFA-enabled store?
status: active
appliesTo:
  - axis: store-setting
    value: multifactorauthentication.storemode=enabled
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /connect/token
  - coordinate: /sign-in
  - coordinate: POST /api/notifications/journal
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: theme=2.61.0-pr-2601; platform=3.1091.0-pr-3144; setting:MultiFactorAuthentication.StoreMode=Enabled (store)
    at: 2026-10-28T15:52:07.164Z
    by: session:56fb99bb
    who: Lenajava1
---
On vcptcore (2026-10-28, theme 2.61.0-pr-2601) the B2B store's MultiFactorAuthentication.StoreMode was set to Enabled. A buyer's password sign-in on /sign-in was challenged: heading "Enter the sign-in code" and one MfaSignInEmailNotification in the journal; entering the code signed the buyer in. The buyer then added a passkey on /account/security (virtual authenticator), signed out, and clicked "Sign in with Passkey" on /sign-in. The page sent POST /connect/token grant_type=passkey with storeId, got 200, and went straight to the home page with the account button showing the buyer. No code step was shown. No MfaSignInEmailNotification was created after the click. The sign-in log after the click held one row, signInType=Passkey, succeeded=true, and no MFA row. So on this build a passkey sign-in counts as enough for store MFA by email.
