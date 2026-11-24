---
id: KB-3CFCC44C
subject: On vcst the storefront /sign-in opens on an OTP-first email form with a "Sign in with a password instead" link; signed-out /account/quotes redirects to /sign-in?returnUrl=/account/quotes
plane: experiential
question: What does the storefront sign-in page show first, and where does a guest on /account/quotes land?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: user
    value: guest
anchors:
  - coordinate: /sign-in
  - coordinate: /account/quotes
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7f9888707d648724
    at: 2026-11-24T07:48:12.367Z
    by: session:7ce2b8e4
    who: Lenajava1
---
2026-11-23 Firefox lane, signed out, theme 2.63.0-pr-2667: /sign-in shows "We'll email you a code. No password needed.", an Email field and Continue, then buttons Sign in with Entra ID / Google / Passkey; the link "Sign in with a password instead" reveals Email + Password + Sign in + "Sign in with a one-time code instead". A guest header has no Organizations control. Opening /account/quotes ends at /sign-in?returnUrl=/account/quotes while the footer still lists "Quote requests".
