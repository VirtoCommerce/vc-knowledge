---
id: KB-7EFFA0AD
subject: With global MFA on, a customer password grant with NO storeId answers two_factor_required and sends the code email; user_cannot_login_in_store is only reached with global off
plane: experiential
question: What does POST /connect/token answer for a customer with no storeId when MultiFactorAuthentication.Enabled is on, and is a code email sent first?
concepts:
  - id: sign-in
status: active
appliesTo:
  - axis: feature
    value: mfa
  - axis: setting
    value: global-mfa-on
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: 594519586290ed24
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.MFA=3.1000.0-pr-1-cf83; setting:MultiFactorAuthentication.Enabled=true
    defect: suspected
    at: 2026-10-22T13:49:40.487Z
    by: session:9b586f75
    who: Lenajava1
    note: "distinct: KB-7A4C260E describes the store rule with MFA absent; this is the order of the MFA challenge against that rule when global MFA is on, plus the store-bound twoFactorToken."
  - method: observation
    deployment: vcst_qa
    build: d70745c02c9df3fe
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.MFA=3.1000.0-pr-1-cf83; setting:MultiFactorAuthentication.Enabled=true/false
    at: 2026-10-23T12:57:58.890Z
    by: session:7ed484a4
    bodyHash: e13e3ab956d9
    who: kutasinaelena
    note: "2026-10-23: global on -> customer no storeId and unknown storeId both two_factor_required; global off -> both user_cannot_login_in_store (own-store customer and a B2B-store customer). The ticket from storeId A refused with storeId B or none as invalid_two_factor_token and is NOT consumed by that refusal (same ticket + storeId A then 200)."
---
On vcst_qa (platform 3.1090.0-pr-3133, MFA 3.1000.0-pr-1-cf83) with global MultiFactorAuthentication.Enabled=true, a created customer of a store with no StoreMode set sent grant_type=password with no storeId: HTTP 400, error invalid_grant, code two_factor_required, a twoFactorToken, providers [Email], and the notification journal gained one MfaSignInEmailNotification row for that customer (0 to 1). The same customer with global off gets user_cannot_login_in_store (KB-7A4C260E). Presenting that kind of twoFactorToken with the mfa grant while naming a different or no store is refused invalid_two_factor_token (the token is bound to the store named at the password step). So the challenge and the email come before the store rule and API clients see a different error code than before the feature (VCST-6353 says responses stay the same).
