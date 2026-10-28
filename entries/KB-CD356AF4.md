---
id: KB-CD356AF4
subject: "Storefront /sign-in auto-fires a passkey conditional-mediation attempt on page load: a grant_type=passkey POST /connect/token runs unprompted, and a stale/removed resident credential returns 400 login_failed plus a visible console error."
plane: experiential
question: Does the storefront /sign-in page attempt a passkey sign-in automatically on load (conditional mediation), and what happens with a stale credential?
status: active
appliesTo:
  - axis: feature
    value: passkey-signin
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: POST /connect/token
  - coordinate: Query.passkeyRequestOptions
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: theme=2.61.0-pr-2601; module:VirtoCommerce.Passkeys=3.1000.0-pr-1; store=B2B-store; platform=3.1091.0-pr-3144
    defect: suspected
    at: 2026-10-28T13:25:01.109Z
    by: session:56fb99bb
    who: Lenajava1
---
On vcptcore QA storefront (theme 2.61.0-pr-2601, Passkeys plugin 1.23.3) loading /sign-in fetches GetPasskeyRequestOptions and the host code runs a WebAuthn conditional-mediation get; when a resident passkey credential exists in the browser but no longer maps to a server-side sign-in key (e.g. after the key was deleted/re-seeded), the browser silently posts grant_type=passkey&scope=offline_access&storeId=B2B-store to /connect/token, which returns HTTP 400 {code:login_failed} and logs a console ERROR 'Failed to load resource: 400 /connect/token' on the public sign-in page. With a valid resident credential the same flow signs the user in with no button press. The passkeys MFE bundle itself contains only navigator.credentials.create (registration); the authentication navigator.credentials.get lives in host/core code.
