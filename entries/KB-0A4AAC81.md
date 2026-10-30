---
id: KB-0A4AAC81
subject: Clicking "Sign in with Passkey" on storefront /sign-in signs a buyer who has a passkey in with no email or password typed, and lands on the home page with the buyer's name in the header
plane: experiential
question: Does the storefront Sign in with Passkey button sign the customer in without typing an email or password?
questions:
  - text: Where does a storefront passkey sign-in land when /sign-in was opened directly?
  - text: Does passkey sign-in on the storefront need the email first?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: ed1246f57f06adbd
    conditions: module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; theme=2.61.0-pr-2608-86ad-86adb577; store=B2B-store
    at: 2026-10-30T13:48:51.509Z
    by: session:b49fdcd3
    who: Lenajava1
---
Signed-out buyer with one passkey registered, Chrome lane, virtual authenticator with presence answered. /sign-in opened directly (no returnUrl) shows the one-time-code view with "Sign in with Passkey" beside the other provider buttons. One click on it, with the email field empty, completes the ceremony and signs the buyer in: the app navigates to / and the header account button shows the buyer's name (AGENT-TEST-PK BUYER) and the organization. No email or password was typed.
