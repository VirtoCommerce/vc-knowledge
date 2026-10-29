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
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: theme=2.61.0-pr-2601-8058-80583415; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; store=B2B-store
    at: 2026-10-28T14:12:04.541Z
    by: session:56fb99bb
    bodyHash: da6bae083a93
    defect: suspected
    who: Lenajava1
    note: "Re-observed twice: a resident credential whose server key was deleted makes /sign-in load POST grant_type=passkey -> 400 login_failed, console error, AND a visible inline alert plus toast 'Login attempt failed. Check your credentials' on a page the user did nothing on; form stays usable. A valid credential signs in silently on load (one Passkey log row). Correction to the body: on this build the authentication navigator.credentials.get (mediation conditional, AbortController) lives in the Passkeys module asset passkey-provider-*.js, not in host code; after a button sign-in it re-arms conditional mediation."
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144; theme=2.61.0-pr-2601; module:VirtoCommerce.Passkeys=3.1000.0-pr-1; store=B2B-store
    at: 2026-10-28T14:55:33.285Z
    by: session:56fb99bb
    bodyHash: da6bae083a93
    who: Lenajava1
    note: "Re-observed in regression REG-2026-10-28-1519 on chrome: valid resident credential -> silent Passkey sign-in on /sign-in load (no click); stale credential (server key deleted via xAPI) -> unprompted POST /connect/token grant_type=passkey 400 invalid_grant/login_failed, console error, inline 'Login attempt failed. Check your credentials' alert; no sign-in-log row for the refused grant; password form stays usable."
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: platform=3.1091.0-pr-3144-9e58; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; theme=2.61.0; store=B2B-store; browser=edge-155 virtual authenticator ctap2 internal resident, automaticPresenceSimulation=true
    at: 2026-10-29T17:28:44.877Z
    by: session:e80d36e0
    bodyHash: da6bae083a93
    who: Lenajava1
    note: "Re-observed on vcst_qa (Edge, virtual authenticator, automaticPresenceSimulation ON): (a) fresh /sign-in load with a valid resident credential: one grant_type=passkey POST /connect/token 200, URL leaves /sign-in, no click; (b) after the server key was deleted from /account/security (authenticator kept the credential): /sign-in load sends POST /connect/token 400 {code:login_failed, error:invalid_grant}, console error 'Failed to load resource 400 /connect/token', and the inline alert 'Login attempt failed. Check your credentials' is visible at 12s; password sign-in afterwards works. Source (passkey-provider.vue onMounted -> signIn mediation conditional; usePasskeySignIn catch -> Logger.error + notifications.error with no conditional-mediation exemption) explains it and is identical on main and a25fea0."
  - method: observation
    deployment: vcst_qa
    build: 1489bcaaf835d77b
    conditions: platform=3.1091.0-pr-3144-9e58; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; theme=2.61.0; store=B2B-store
    at: 2026-10-29T18:08:59.511Z
    by: session:e80d36e0
    bodyHash: da6bae083a93
    who: Lenajava1
    note: "vcst_qa 2026-10-29 Edge, presence ON: anonymous /account/security -> guard -> /sign-in?returnUrl=/account/security; with a valid resident credential the load-time request signed the buyer in with no click (one grant_type=passkey 200, one Passkey log row) and returned to /account/security. The same happens straight after signing out while on a protected page."
---
On vcptcore QA storefront (theme 2.61.0-pr-2601, Passkeys plugin 1.23.3) loading /sign-in fetches GetPasskeyRequestOptions and the host code runs a WebAuthn conditional-mediation get; when a resident passkey credential exists in the browser but no longer maps to a server-side sign-in key (e.g. after the key was deleted/re-seeded), the browser silently posts grant_type=passkey&scope=offline_access&storeId=B2B-store to /connect/token, which returns HTTP 400 {code:login_failed} and logs a console ERROR 'Failed to load resource: 400 /connect/token' on the public sign-in page. With a valid resident credential the same flow signs the user in with no button press. The passkeys MFE bundle itself contains only navigator.credentials.create (registration); the authentication navigator.credentials.get lives in host/core code.
