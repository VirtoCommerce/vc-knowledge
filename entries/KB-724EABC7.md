---
id: KB-724EABC7
subject: Admin Shell sign-in of a Manager whose own store requires MFA completes via the cookie login, but its parallel /connect/token password grant answers 400 two_factor_required, emails an unrequested "Your sign-in code" and logs the twoFactorToken to the browser console
plane: experiential
question: Does the Admin Shell sign-in send an MFA sign-in code for a Manager whose own store (storeId) is in MultiFactorAuthentication.StoreMode Enabled while global MFA is off?
questions:
  - text: Why does a Backoffice user get a sign-in code email when they log into the Admin Shell?
  - text: Admin Shell fetchToken HTTP error 400 two_factor_required
  - text: Does the AngularJS Backoffice login ask for MFA when the user's store is Enabled?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /connect/token
  - coordinate: /apps/admin-shell#/login
  - coordinate: POST /api/platform/security/login
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; module:VirtoCommerce.AdminShell=3.1093.0; setting:MultiFactorAuthentication.Enabled=false; setting:MultiFactorAuthentication.StoreMode=Enabled (user's own store); role=Manager with mfa:manage, not administrator
    defect: suspected
    at: 2026-11-04T14:26:25.617Z
    by: session:7f21950d
    who: Lenajava1
    note: "distinct: Those entries are about a CUSTOMER's storefront password grant; this one is the Admin Shell (Backoffice) sign-in of a MANAGER with global MFA off: the cookie login succeeds without a code while the shell's own token grant mails an unrequested code and logs the 2FA ticket to the console."
---
Global MultiFactorAuthentication.Enabled false. A non-administrator Manager (roles: platform:security:access, platform:security:read, mfa:read, mfa:manage) with user MFA mode Required and storeId bound to a disposable store whose StoreMode is Enabled. AngularJS Backoffice sign-in (#!/login): POST /api/platform/security/login 200 {succeeded:true, requiresTwoFactor:false}, workspace opens, no code step, 0 MfaSignInEmailNotification rows. Admin Shell sign-in (/apps/admin-shell/#/login, same account): POST /api/platform/security/login 200, then POST /connect/token grant_type=password (no storeId) 400 {code: two_factor_required, twoFactorToken, provider Email}; the shell still lands on its dashboard with no code prompt, the console logs "[@vc-shell/framework#use-user] fetchToken - HTTP error: 400" with the full twoFactorToken, and the notification journal gains one MfaSignInEmailNotification "Your sign-in code" at the sign-in second. BL-AUTH-025 says store and user modes never add the code on the Backoffice sign-in; the UI is not challenged, but the code is still generated and mailed.
