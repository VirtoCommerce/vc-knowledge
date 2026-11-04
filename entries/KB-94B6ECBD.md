---
id: KB-94B6ECBD
subject: On /account/security the two-factor widget shows a switch only in a CustomerChoice store for a user nobody else requires it of; every other state is read-only with its own reason line
plane: experiential
question: What does the two-factor authentication widget on /account/security show in each store mode and for each reason MFA is required?
questions:
  - text: When does a storefront customer see Turn on / Turn off for two-factor authentication on Account Security?
  - text: What text does Account Security show when the administrator set the user's MFA mode to Required?
  - text: What does Account Security show in an AdminManaged store?
  - text: What does a member of an organization that requires MFA see on Account Security?
  - text: What does Account Security show for an impersonated (Login on behalf) session?
  - text: What does Account Security show to a customer whose email is not confirmed?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/security
  - coordinate: Query.mfaSettings
  - coordinate: Mutation.requestMfaChangeCode
  - coordinate: Mutation.updateMfaUserMode
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; theme=2.62.0-pr-2622-caac-caac275a; setting:MultiFactorAuthentication.Enabled=false
    at: 2026-11-04T14:24:24.712Z
    by: session:7f21950d
    who: Lenajava1
---
Live, 2026-11-04, theme 2.62.0-pr-2622 + MFA 3.1002.0-pr-3, the storefront store switched through StoreMode values. CustomerChoice, user Off: status "Off" / "Signing in needs only your password." and a "Turn on" button; Turn on opens an inline code step ("To turn on two-factor authentication, enter the code we sent to <masked>", Code field auto-submits at 6 digits, Confirm, "Resend code in N s", Cancel); a wrong code shows the alert "That code didn't work."; Resend after the countdown shows "New code sent."; once confirmed the status reads "On" / "You turned it on: we send a code to <masked> every time you sign in." with a "Turn off" button (same code step to turn off). User Required by the admin: "On" / "The administrator turned it on for your account: …" plus "Set by the administrator: you can't turn it off." and no button. Organization requires it (member, own mode Off): "On" / "Your company requires it: …" plus "Your company turned it on for all its members, so you can't change it here." and no button. Login on behalf: status shown plus "You can't change it while you act on behalf of the customer." and no button. Unconfirmed email: "Turn on" is disabled with "Confirm your email to turn on two-factor authentication."; if the admin sets such a user to Enabled, the widget reads "On" with an empty destination ("we send a code to  every time…") and an enabled "Turn off" that answers "We can't send a sign-in code to this account. Please contact your administrator." (requestMfaChangeCode mfa_provider_not_available), and the password sign-in shows the same message (account cannot sign in). AdminManaged: status plus "The store administrator manages two-factor authentication." and no button (Off user and Required user alike). Store Enabled: "On" / "The store requires it: we send a code to <masked> every time you sign in." and no button.
