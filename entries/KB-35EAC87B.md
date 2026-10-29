---
id: KB-35EAC87B
subject: A storefront passkey sign-in started from /sign-in?returnUrl=/account/security returns the buyer to /account/security, on the button path and on the load-time autofill path
plane: experiential
question: "storefront /sign-in?returnUrl after sign-in: does the app return to the page the user came from after a passkey sign-in?"
questions:
  - text: After 'Sign in with Passkey' from a protected page, where does the storefront land?
  - text: Does the passkey sign-in honour returnUrl or go to the dashboard/home page?
  - text: Does the auth guard redirect to /sign-in carry a returnUrl for /account/security?
  - text: Does the autofill (conditional mediation) passkey sign-in return to the protected page?
status: active
appliesTo:
  - axis: feature
    value: passkey-signin
  - axis: grant_type
    value: passkey
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in?returnUrl
  - coordinate: POST /connect/token
  - coordinate: /account/security
  - coordinate: Query.passkeyRequestOptions
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: platform=3.1091.0-pr-3144-9e58; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; module:XFrontend=3.1008.0-alpha.64-vcst-6121-passkeys; theme=2.61.0; store=B2B-store; browser=edge-155 virtual authenticator
    at: 2026-10-29T17:55:43.724Z
    by: session:e80d36e0
    who: Lenajava1
---
Edge lane, WebAuthn virtual authenticator (ctap2, internal, resident, UV), B2B buyer on store B2B-store with one passkey registered on /account/security. (1) Button path: anonymous visitor opens /account/security with automaticPresenceSimulation OFF; the auth guard lands on /sign-in?returnUrl=/account/security showing 'Sign in with Passkey'. Presence switched ON, button clicked: one POST /connect/token grant_type=passkey (200, no Authorization header, referer /sign-in?returnUrl=/account/security), then the browser ends on /account/security with heading 'Security' and the account menu naming the buyer and organization. The re-armed GetPasskeyRequestOptions query follows (pending). Sign-in log search after T0 holds exactly one succeeded Passkey row, storeId B2B-store, host the storefront host. (2) Autofill path (incidental): signing out while on /account/security with presence ON sends the browser through the guard to /sign-in?returnUrl=/account/security, the load-time conditional request is answered by the virtual authenticator, POST /connect/token grant_type=passkey 200 (referer /sign-in?returnUrl=/account/security), and the browser lands on /account/security signed in again (GA login event method=passkey, dr=/sign-in?returnUrl=/account/security, dl=/account/security). So the returnUrl is honoured on both passkey paths. Consequence for tests: setting presence OFF only after the sign-out click is too late on a protected page; switch it off before signing out.
