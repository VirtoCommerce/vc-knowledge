---
id: KB-6B10CB0E
subject: The Admin "Store authentication types" blade lists only Password and the external IdPs (no OTP), while the xAPI authenticationTypes and the storefront buttons follow different lists per stand; the storefront drops the Virto IdP
plane: experiential
question: Which sign-in methods does the store Authentication blade control, and what does the storefront render from store.settings.authenticationTypes?
status: active
appliesTo:
  - axis: feature
    value: authentication-types
  - axis: surface
    value: admin-spa
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.store
  - coordinate: GET /api/platform/settings/OtpSignIn.Enabled
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: db5e6e6e527dec12
    conditions: platform=3.1087.0-pr-3134-a3d2; module:VirtoCommerce.OTP=3.1000.0; module:VirtoCommerce.Store=3.1010.0; module:VirtoCommerce.OpenIdConnectModule=3.1001.0; theme=2.61.0-pr-2564-2fa8
    at: 2026-10-21T16:33:23.364Z
    by: session:5fa7c31a
    who: Lenajava1
---
On vcptcore_qa1 the Store > Authentication widget opens "Store authentication types" with toggles for Password, Google, Microsoft Entra ID and Virto (all active) and no OTP entry; OTP sign-in is switched only by Settings > OTP Sign-In > General > "Enable OTP sign-in" (setting OtpSignIn.Enabled, group OTP Sign-In|General, public). Anonymous xAPI store(storeId:"B2B-store").settings.authenticationTypes returned ["Password","GoogleSSO","AzureAD","virto"] there and ["Password","AzureAD","GoogleSSO"] on vcst_qa; it never lists OTP. The qa1 storefront /sign-in nonetheless shows only "Sign in with Google" and "Sign in with Entra ID" (no Virto button) and the buttons follow the xAPI order, while the qa1 Admin login page offers Microsoft Entra ID, Google and Virto and the vcst_qa Admin login page offers Microsoft Entra ID and "Google sign-in".
