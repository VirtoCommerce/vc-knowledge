---
id: KB-3BF84A5E
subject: POST /connect/token grant_type=mfa redeems the two-factor ticket on a right code BEFORE the store rule refuses, so a retry with the same ticket and code answers invalid_two_factor_token and the user must redo the password step for a new emailed code
plane: experiential
question: Is the MFA code or ticket spent when a validator after the code (store rule) refuses the sign-in, and what does a retry return?
questions:
  - text: Does user_cannot_login_in_store come before or after the MFA code step?
  - text: Can a buyer retry the same MFA code after a store refusal?
  - text: Which failure reason does the sign-in log record when the store rule refuses after the MFA code?
status: active
appliesTo:
  - axis: feature
    value: mfa
  - axis: grant
    value: mfa
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: d70745c02c9df3fe
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.MFA=3.1000.0-pr-1-cf83; setting:MultiFactorAuthentication.Enabled=false; setting:MultiFactorAuthentication.StoreMode=Enabled (requested store)
    at: 2026-10-23T11:42:55.259Z
    by: session:7ed484a4
    who: kutasinaelena
    note: "distinct: KB-7EFFA0AD is about the password step with no storeId under global on; KB-C196A782 about a bogus token. This entry is about a REAL ticket redeemed by a right code and then refused by the store rule (StoreMode Enabled, global off) - ticket spent, retry refused, log reason Unknown."
---
On vcst_qa a customer account belonging to store B was signed in with grant_type=password and storeId of a store A whose MultiFactorAuthentication.StoreMode=Enabled (global setting off). The password step answered 400 two_factor_required with a twoFactorToken and an MfaSignInEmailNotification was written to the journal - the store rule did not refuse at the password step. The right code on grant_type=mfa answered 400 invalid_grant code=user_cannot_login_in_store. Replaying the same two_factor_token + same code (with storeId A, and with storeId B) both answered 400 invalid_two_factor_token, so the ticket was redeemed before the store rule ran. A fresh password step issued a new ticket and a new code; the first code on it answered invalid_code and incremented accessFailedCount. The sign-in log recorded the refused MFA row with failureReason Unknown (not the store-rule reason) and storeId = the account's own store B, not the requested store A.
