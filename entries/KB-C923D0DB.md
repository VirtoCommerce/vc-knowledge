---
id: KB-C923D0DB
subject: the email confirmation link works without a session and does not sign the user in
plane: experiential
question: What happens when a user opens the /account/confirmemail link, and how is a confirmation email resent?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/confirmemail
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:17:26.163Z
    by: session:memimpor
    who: Lenajava1
---
The confirmation email's link points to /account/confirmemail with the user id and an identity token; opened in a fresh browser with no session it renders the Email confirmation success page asking the user to sign in, and does not sign them in. Opening a freshly resent link for an already-confirmed account shows the same success page rather than an error. The storefront /sign-in page has no self-service resend link, BY DESIGN; the resend path is the Admin user blade's Resend link, which produces an Email confirmation notification visible in the Admin notification activity feed with a preview of the body.
