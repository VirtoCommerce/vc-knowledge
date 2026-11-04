---
id: KB-8E161EEC
subject: With global MultiFactorAuthentication.Enabled=true, a back-office Manager is challenged (400 two_factor_required, one code email) on the password grant even when it names a StoreMode=Disabled store, and equally without a storeId
plane: experiential
question: Does a StoreMode=Disabled store waive the MFA code for a Manager (Backoffice) account that names it in the password grant while global MFA is on?
questions:
  - text: Is a back-office Manager asked for the MFA code on /connect/token with storeId of a Disabled store?
  - text: Does StoreMode Disabled skip MFA for Backoffice users?
  - text: Does a Manager get tokens without a code when naming a Disabled store (VCST-6530 behaviour)?
status: active
appliesTo:
  - axis: account
    value: manager-backoffice
  - axis: grant
    value: password-with-storeid
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.Enabled=true; setting:MultiFactorAuthentication.StoreMode=Disabled
    at: 2026-11-04T15:43:28.163Z
    by: session:7f21950d
    who: Lenajava1
    note: "distinct: The held entries cover customers (the store's own customer, a foreign-store customer, a store-less customer); this one is a back-office Manager account (userType Manager), the Backoffice branch of the Disabled-store rule."
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.Enabled=true; setting:MultiFactorAuthentication.StoreMode=Disabled
    at: 2026-11-04T20:08:01.570Z
    by: session:2f6b8998
    bodyHash: 13176fb8ea1d
    who: Lenajava1
    note: "Independent judge global window 19:58:21-19:58:27Z: a Manager bound to a Disabled run store naming it, and a store-less Manager naming it, each answered two_factor_required; a customer with a permission-bearing run role on its own Disabled store too (VCST-6530 fix holds)."
---
2026-11-04 on vcst_qa (platform 3.1094.0, VirtoCommerce.MFA 3.1002.0-pr-3-1a0f). Global MultiFactorAuthentication.Enabled written true and read back true for ~3 s. A disposable store cloned from the B2B store with MultiFactorAuthentication.StoreMode=Disabled (read back). A Manager account (userType Manager, a back-office role in user.Roles, no storeId, confirmed email, twoFactorEnabled false). POST /connect/token grant_type=password, scope=offline_access, storeId=<the Disabled store> answered HTTP 400 error=invalid_grant, errors[0].code=two_factor_required, twoFactorToken present, no access_token; exactly one MfaSignInEmailNotification journal row appeared (0 -> 1, ~0.6 s). The same request without storeId: also 400 two_factor_required with twoFactorToken (journal 2 rows settled). In the same window the Disabled store's own role-less confirmed customer, same request with storeId, got 200 with tokens and no code. So the Disabled store skips the code only for its own non-Backoffice customers; a Manager follows the global setting whether or not it names the store (the 3.1000.x behaviour where a named Disabled store let a Manager in without a code is gone on this build). No stack trace, no 5xx.
