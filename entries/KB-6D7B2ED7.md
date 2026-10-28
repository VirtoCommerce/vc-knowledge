---
id: KB-6D7B2ED7
subject: Storefront OTP sign-in form posts to the storefront origin /api/otp/request; where the storefront ingress has no route for it the answer is a 405 HTML page and the form shows a generic error
plane: experiential
question: What happens when a guest clicks Continue on the storefront OTP email sign-in form?
questions:
  - text: Why does the email code login fail with 'Something went wrong' on the storefront?
  - text: POST /api/otp/request 405 storefront origin ingress Cloudflare
  - text: Does the OTP request from /sign-in reach the platform or is it blocked at the storefront ingress?
  - text: "Which errors appear together when the one-time code Continue fails: inline alert and server-error toast?"
  - text: What validation message does the OTP email field show for 'john@' or an empty email?
concepts:
  - id: otp
  - id: sign-in
  - id: api-error
status: active
appliesTo:
  - axis: feature
    value: otp-email-sign-in
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/otp/request
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T22:34:59.318Z
    by: session:238f2094
    who: kutasinaelena
    build: 1212305283171b63
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T06:10:31.424Z
    by: session:238f2094
    who: kutasinaelena
    contradicts: true
    note: "Superseded the same day once the ingress route was added: POST {storefront}/api/otp/request now returns 200 {succeeded:true, error:null, maskedEmail:\"t•••0@test-agent.com\"} and the form moves to the \"Check your email\" step (\"We sent a code to t•••0@...\"). The 405 was a deployment ingress gap, not product behaviour; the client-side validation part of the entry still holds."
    resolved: conditions-scoped
    resolvedAt: 2026-10-08T10:59:48.783Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/otp-signin-20261008
    resolution: "The 405 depends on whether the storefront ingress routes /api/otp: routed on vcptcore_qa1 now (200), not routed on vcst_qa (405) - both live 2026-10-08. Subject and body scoped by that condition."
    build: 1212305283171b63
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; module:VirtoCommerce.OTP=3.1000.0; module:VirtoCommerce.Store=3.1009.0; theme=2.59.0-pr-2532-c725; store=B2B-store; setting:OtpSignIn.Enabled=true; ingress:/api/otp=routed
    at: 2026-10-08T10:59:44.837Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: /sign-in opens on the code view; empty and malformed emails are refused client-side with no request; Continue POSTs {storefront}/api/otp/request -> 200 JSON succeeded:true, maskedEmail a•••1@example.com, and the form shows Check your email; the same POST to the platform host answers identically."
    investigation: true
    build: 7592346292e2fa35
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.OTP=3.1000.0; module:VirtoCommerce.Store=3.1009.0; theme=2.59.0; store=B2B-store; setting:OtpSignIn.Enabled=false; ingress:/api/otp=not routed
    at: 2026-10-08T10:59:45.635Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: POST {storefront}/api/otp/request -> 405 text/html \"405 Not Allowed\" (nginx behind Cloudflare); the same POST to the platform host -> 200 JSON error otp_disabled. The storefront shows only the password form because OTP is off for the store."
    investigation: true
    build: c4000d0bea152759
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:55:45.492Z
    by: session:df9d131f
    who: Lenajava1
    note: "Same ingress gap present on vcst_qa 2026-10-09 (theme vc-frontend-next 3.0.0-alpha.2685, OTP module 3.1000.0, OtpSignIn.Enabled temporarily on): POST {storefront}/api/otp/request -> 405 nginx HTML, form shows inline 'Something went wrong. Please try again later.'; the identical body POSTed to the platform host returned 200 succeeded:true and the OtpSignInEmailNotification was journaled. GET {storefront}/api/otp/request returns the SPA index, while {storefront}/api/files/* is proxied - the storefront proxy whitelists paths and lacks /api/otp. Deployment-specific, not product behaviour."
    build: a6e179f178104063
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T13:56:24.196Z
    by: session:df9d131f
    who: Lenajava1
    contradicts: true
    note: "Superseded on vcst_qa 2026-10-09 ~13:53 UTC once the storefront ingress gained a /api/otp route to the platform (theme vc-frontend-next 3.0.0-alpha.2685, OtpSignIn.Enabled on): clicking Continue on /sign-in sent POST {storefront}/api/otp/request {email, storeId} -> 200 application/json {succeeded:true, error:null, maskedEmail, lockoutSecondsRemaining:null} in ~0.5s, the form moved to the \"Check your email\" code step with the masked address, the OtpSignInEmailNotification was journaled and sent, and entering the code auto-submitted POST {storefront}/connect/token grant_type=otp_email -> 200 and signed the user in. As on vcptcore_qa1, the 405 was a deployment ingress gap; with the route present the flow works end to end."
    resolved: conditions-scoped
    resolvedAt: 2026-10-13T05:50:16.006Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "Both sides are right under their stand's ingress: vcst routes /api/otp since 2026-10-09 and answers JSON (2026-10-13); vcptcore-qa has no route (nginx 405) and no OTP module; the entry already names the route as the condition."
    build: a6e179f178104063
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1078.0; module:VirtoCommerce.OTP=3.1000.0; setting:OtpSignIn.Enabled=true; ingress:/api/otp=routed
    at: 2026-10-13T05:50:09.106Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-13, 2 runs: POST {storefront}/api/otp/request unknown email -> 200 application/json user_not_found, identical to the platform host."
    investigation: true
    bodyHash: e517fd4d18a1
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    conditions: ingress:/api/otp=not routed; state:otp-module=absent
    at: 2026-10-13T05:50:10.929Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-13, 2 runs: POST {storefront}/api/otp/request -> 405 text/html nginx; GET -> SPA index; platform host POST/GET -> 404; no VirtoCommerce.OTP in the vcptcore-qa manifest."
    investigation: true
    bodyHash: e517fd4d18a1
    build: c55c1a124b302d3b
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144; theme=2.61.0-pr-2601-8058; module:VirtoCommerce.OTP=3.1001.0; store=B2B-store; setting:OtpSignIn.Enabled=true; ingress:/api/otp=routed
    at: 2026-10-28T16:01:52.029Z
    by: session:56fb99bb
    bodyHash: e517fd4d18a1
    who: Lenajava1
    note: "2026-10-28: vcptcore_qa now carries the OTP module and routes the storefront /api/otp (POST answers 200 application/json). With OtpSignIn.Enabled on, /sign-in opened on the code view at the first load after the setting PUT (no cache lag), Continue moved to 'Check your email' with a masked address and six code cells, the OtpSignInEmailNotification was journaled ~3s later, and typing the code auto-submitted and signed in (sign-in log signInType OTP, succeeded)."
---
With OtpSignIn.Enabled on the store, /sign-in opens on the one-time-code view ("We'll email you a code. No password needed.", Email, Continue, "Sign in with a password instead"). Continue validates the email client-side - empty: "This field is required"; "john@" or an address with a space: "Enter a valid email address, e.g. johndoe@gmail.com" - and sends nothing until it is valid. Then it POSTs /api/otp/request RELATIVE TO THE STOREFRONT ORIGIN, so it only reaches the platform if the storefront's ingress routes /api/otp to it.

Where it does (vcptcore_qa1 from 2026-10-01; observed live 2026-10-08): 200 {succeeded:true, maskedEmail:"a•••1@example.com"} - the mask keeps the first and last character of the local part - and the form moves to "Check your email". An unknown email gets 200 with error code user_not_found; a store with OTP off gets error code otp_disabled.

Where it does not (vcptcore_qa1 before 2026-10-01; vcst_qa on 2026-10-08): the storefront ingress answers 405 text/html "405 Not Allowed" (nginx) and the request never reaches the platform, while the same POST to the platform's own host answers JSON. On that 405 the form showed an inline "Something went wrong. Please try again later." plus the global server-error toast (observed 2026-09-30). So a 405 here is a deployment's ingress gap, not product behaviour - check the route before reading the form's error as an OTP failure. Which case a stand is in changes with its ingress: vcst gained a storefront /api/otp route on 2026-10-09 (~13:53Z), and on 2026-10-13 POST {storefront}/api/otp/request answered 200 application/json (an unknown email: error code user_not_found), the same as the platform host, with OtpSignIn.Enabled on for B2B-store. vcptcore-qa on 2026-10-13 had no route (POST {storefront}/api/otp/request -> 405 text/html nginx, GET -> the SPA index) and no OTP module at all (platform host 404; absent from its deploy manifest).
