---
id: KB-DFC1D68D
subject: A signed-in storefront customer who goes Back to /sign-in after a code sign-in is redirected to /catalog, stays signed in, and no token request is sent
plane: experiential
question: What happens when a signed-in storefront customer presses browser Back onto /sign-in after signing in with an emailed code?
questions:
  - text: Where does /sign-in redirect an already signed-in customer?
  - text: Does browser Back after OTP sign-in show the code step again or resend the code?
status: active
appliesTo:
  - axis: feature
    value: otp-email-sign-in
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: theme=2.63.0-pr-2666-7e02-7e022f10; store=B2B-store; setting:OtpSignIn.Enabled=true
    at: 2026-11-18T14:54:56.139Z
    by: session:6da8340c
    who: Lenajava1
---
On vcst_qa (theme 2.63.0-pr-2666, B2B-store, OTP sign-in on) a customer opened /sign-in from /, signed in with an emailed code (6th digit auto-submits POST /connect/token grant_type=otp_email, 200) and landed on / after a full page load. Pressing browser Back went to /sign-in, which immediately redirected to /catalog; the header still showed the signed-in customer, no code step or used code was rendered, and the session network log held only the one /connect/token request. Observed once in a regression run (suite 104 OTP-009).
