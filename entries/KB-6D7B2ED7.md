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
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; module:VirtoCommerce.OTP=3.1000.0; module:VirtoCommerce.Store=3.1009.0; theme=2.59.0-pr-2532-c725; store=B2B-store; setting:OtpSignIn.Enabled=true; ingress:/api/otp=routed
    at: 2026-10-08T10:59:44.837Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: /sign-in opens on the code view; empty and malformed emails are refused client-side with no request; Continue POSTs {storefront}/api/otp/request -> 200 JSON succeeded:true, maskedEmail a•••1@example.com, and the form shows Check your email; the same POST to the platform host answers identically."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.OTP=3.1000.0; module:VirtoCommerce.Store=3.1009.0; theme=2.59.0; store=B2B-store; setting:OtpSignIn.Enabled=false; ingress:/api/otp=not routed
    at: 2026-10-08T10:59:45.635Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: POST {storefront}/api/otp/request -> 405 text/html \"405 Not Allowed\" (nginx behind Cloudflare); the same POST to the platform host -> 200 JSON error otp_disabled. The storefront shows only the password form because OTP is off for the store."
---
With OtpSignIn.Enabled on the store, /sign-in opens on the one-time-code view ("We'll email you a code. No password needed.", Email, Continue, "Sign in with a password instead"). Continue validates the email client-side - empty: "This field is required"; "john@" or an address with a space: "Enter a valid email address, e.g. johndoe@gmail.com" - and sends nothing until it is valid. Then it POSTs /api/otp/request RELATIVE TO THE STOREFRONT ORIGIN, so it only reaches the platform if the storefront's ingress routes /api/otp to it.

Where it does (vcptcore_qa1 from 2026-10-01; observed live 2026-10-08): 200 {succeeded:true, maskedEmail:"a•••1@example.com"} - the mask keeps the first and last character of the local part - and the form moves to "Check your email". An unknown email gets 200 with error code user_not_found; a store with OTP off gets error code otp_disabled.

Where it does not (vcptcore_qa1 before 2026-10-01; vcst_qa on 2026-10-08): the storefront ingress answers 405 text/html "405 Not Allowed" (nginx) and the request never reaches the platform, while the same POST to the platform's own host answers JSON. On that 405 the form showed an inline "Something went wrong. Please try again later." plus the global server-error toast (observed 2026-09-30). So a 405 here is a deployment's ingress gap, not product behaviour - check the route before reading the form's error as an OTP failure.
