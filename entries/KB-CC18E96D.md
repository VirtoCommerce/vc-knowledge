---
id: KB-CC18E96D
subject: The MFA module's switches are a global Boolean (Settings > Platform > Security > Multi-factor authentication, default off) and a per-store dropdown (Store > Settings > Store > Security, default Use global setting); neither is exposed anonymously through xAPI
plane: experiential
question: Where is multi-factor authentication switched on, and can the storefront or an anonymous client read whether a store requires it?
status: active
appliesTo:
  - axis: feature
    value: mfa
  - axis: surface
    value: admin-spa
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: GET /api/platform/settings/MultiFactorAuthentication.Enabled
  - coordinate: GET /api/stores/{id}
  - coordinate: Query.store
evidence:
  - method: observation
    deployment: vcst_qa
    build: c65e39a5daec993f
    conditions: platform=3.1088.0-pr-3133-23a5-vcst-5904; module:VirtoCommerce.MFA=3.1000.0-pr-1-41b3; module:VirtoCommerce.Xapi=3.1027.0
    at: 2026-10-21T16:32:43.199Z
    by: session:5fa7c31a
    who: Lenajava1
---
On vcst_qa (VirtoCommerce.MFA 3.1000.0-pr-1-41b3) GET /api/platform/settings/MultiFactorAuthentication.Enabled returned group Platform|Security, valueType Boolean, value false (default false), and GET /api/stores/{id} listed MultiFactorAuthentication.StoreMode with default "Use global setting" and value null on all seven stores. In the Admin SPA the same switches render as Settings > Platform > Security > "Multi-factor authentication" (toggle) and, in a store's Settings widget, Store > Security > "Multi-factor authentication" (dropdown showing "Use global setting"). The anonymous xAPI query store(storeId) settings.modules lists VirtoCommerce.MFA with no settings at all, while VirtoCommerce.OTP exposes OtpSignIn.Enabled publicly, so the storefront cannot read the MFA requirement ahead of time: it learns it only when the password grant answers two_factor_required. With the global setting off, a password grant for a B2B contact (storeId B2B-store), a B2C contact and the platform admin all returned 200 with no twoFactorToken.
