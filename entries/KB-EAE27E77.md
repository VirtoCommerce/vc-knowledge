---
id: KB-EAE27E77
subject: A storefront customer whose Identity flag twoFactorEnabled=true is challenged with an emailed code on password sign-in even when global MultiFactorAuthentication.Enabled is off and the store has no StoreMode override
plane: experiential
question: Does per-user twoFactorEnabled=true make a storefront password grant ask for an email code when neither the global MFA setting nor the store's StoreMode is enabled?
questions:
  - text: How do I get a storefront user to see the MFA code step without enabling MFA for the whole store?
  - text: Does POST /connect/token answer two_factor_required for a user with twoFactorEnabled when MFA settings are off?
  - text: Is the per-user two-factor flag honoured by the MFA module on a customer sign-in?
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: user-flag
    value: twofactorenabled
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 023779f193646d5e
    conditions: module:VirtoCommerce.MFA=3.1000.1; setting:MultiFactorAuthentication.Enabled=unset; setting:MultiFactorAuthentication.StoreMode=unset (store)
    at: 2026-11-02T12:57:16.844Z
    by: session:75d360f6
    who: Aleksandra-Mitricheva
    note: "distinct: KB-B9DB6FC4 is about passkey sign-in with StoreMode=Enabled; this is about a password sign-in with every MFA setting OFF, where only the per-user twoFactorEnabled flag triggers the challenge."
---
With global MultiFactorAuthentication.Enabled unset (default false) and the B2B store's MultiFactorAuthentication.StoreMode unset (null), a new Customer account bound to that store with emailConfirmed=true and twoFactorEnabled=true sent POST /connect/token grant_type=password with storeId. Answer: 400 invalid_grant, code two_factor_required, a twoFactorToken, provider Email. A MfaSignInEmailNotification row appeared in the notification journal (POST /api/notifications/journal) within a second, its 6-digit code readable from the rendered body even though the send status was Error (no mail provider). The mfa grant with that code returned 200 with access and refresh tokens and amr=mfa. So a per-user flag is enough to exercise the storefront code step without changing any shared store or global setting.
