---
id: KB-CA2AE929
subject: An invalid /punchout/&lt;token&gt; start page silently lands the visitor on the anonymous homepage; the grant answers 400 login_failed
plane: experiential
question: What does a buyer see when the punchout start-page token is invalid or expired?
questions:
  - text: What does a buyer see if the punchout link from their purchasing system has expired?
  - text: /punchout/:sessionToken invalid token POST /connect/token grant_type=punchout login_failed
  - text: Does an invalid punchout start page token show an error message on the storefront?
  - text: What does the punchout grant return when session_token is missing?
concepts:
  - id: sign-in
  - id: access-token
  - id: api-error
status: active
appliesTo:
  - axis: module
    value: virtocommerce.punchout
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: /punchout/:sessionToken
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T20:50:49.197Z
    by: session:527ae7ed
    who: kutasinaelena
---
On vcptcore_qa1 (vc-frontend 2.59.0-pr-2506) opening /punchout/<43-char unknown token> with Punchout.Enabled true on the store triggered POST /connect/token grant_type=punchout (400 login_failed, 'Login attempt failed. Please check your credentials.') and the browser then ended on '/' (B2B-store Homepage) as an anonymous visitor with no on-page message; the only trace is a failed resource entry in the console. The grant also returns the same 400 login_failed when session_token is absent.
