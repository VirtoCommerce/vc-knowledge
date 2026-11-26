---
id: KB-BE86FD34
subject: An anonymous visit to /account, /account/orders or /company/members redirects to /sign-in?returnUrl=<path>, and a password sign-in returns to that path
plane: experiential
question: Where does the storefront send an anonymous visitor who opens an account page, and does sign-in return them there?
questions:
  - text: Does /sign-in keep a returnUrl for protected account routes?
  - text: Is /company/members behind a login gate for anonymous visitors?
  - text: What returnUrl does /account produce on the sign-in redirect?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: /account/orders
  - coordinate: /company/members
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; platform=3.1101.0; store=B2B-store
    at: 2026-11-26T00:15:38.261Z
    by: session:779b94fc
    who: kutasinaelena
---
Fresh signed-out context, theme 2.63.0-alpha.2681, store B2B-store. Opening /account lands on /sign-in?returnUrl=/account/dashboard; /account/orders lands on /sign-in?returnUrl=/account/orders; /company/members lands on /sign-in?returnUrl=/company/members. The redirect is client-side (SPA), the sign-in form opens on the one-time-code view with a 'Sign in with a password instead' switch, and no account data renders. Signing in with a password from /sign-in?returnUrl=/account/orders lands directly on /account/orders (Orders heading), POST /connect/token 200.
