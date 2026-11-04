---
id: KB-AA8E794C
subject: With global MultiFactorAuthentication.Enabled=true, a password grant naming a StoreMode=Disabled store is challenged (400 two_factor_required, one code email) for a customer bound to a different store, while the Disabled store's own customer gets 200 with tokens and no code
plane: experiential
question: Does a StoreMode=Disabled store waive the MFA code for a customer of another store who names it in the password grant while global MFA is on?
questions:
  - text: Is the MFA StoreMode Disabled exemption limited to the store's own customers?
  - text: What does /connect/token return for a foreign-store customer naming a Disabled-MFA store with global MFA enabled?
  - text: Does the MFA challenge come before the store sign-in refusal for a user of another store?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.mfa
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: setting:MultiFactorAuthentication.Enabled=true; setting:MultiFactorAuthentication.StoreMode=Disabled (named store); account=role-less customer, emailConfirmed=true
    at: 2026-11-04T15:30:51.061Z
    by: session:7f21950d
    who: Lenajava1
    note: "distinct: KB-0A3256BC is global off + StoreMode Enabled; KB-7EFFA0AD is a grant with no storeId; KB-0929D604 (now disputed) is the pre-VCST-6530 build where Disabled waived the code for every user — this is the narrowed behaviour on the current build: a customer of ANOTHER store naming a Disabled store is challenged."
---
2026-11-04 on vcst_qa, global MultiFactorAuthentication.Enabled set true (read back) for ~10 s. A disposable store cloned from the B2B store with MultiFactorAuthentication.StoreMode=Disabled (read back). Two role-less confirmed-email customers of the same kind: one bound to the Disabled store, one bound to the shared B2B store. Both sent POST /connect/token grant_type=password, scope=offline_access, storeId=<the Disabled store>. Own customer: HTTP 200 with access_token and refresh_token, no twoFactorToken, zero MfaSignInEmailNotification journal rows. Foreign customer: HTTP 400 error=invalid_grant, errors[0].code=two_factor_required, a twoFactorToken, no access_token, and exactly one new MfaSignInEmailNotification journal row (status Pending). So Disabled waives the code only for the store's own customers; a user of another store follows the global setting, and the MFA challenge is answered before any store sign-in refusal (no user_cannot_login_in_store). No stack trace in either body.
