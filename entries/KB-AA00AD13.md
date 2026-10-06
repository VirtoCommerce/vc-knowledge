---
id: KB-AA00AD13
subject: Opening the email confirmation link shows a success page without signing the user in
plane: experiential
question: What happens when a user opens the /account/confirmemail link?
questions:
  - text: After clicking the verify link in my email, why am I asked to sign in instead of being logged in?
  - text: Does the email confirmation page need an existing session to work?
  - text: What does the confirm-email route show for an account that is already confirmed?
  - text: Which parameters does the confirmation link carry to the storefront page?
concepts:
  - id: email-confirmation
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/confirmemail
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:17:26.163Z
    by: session:memimpor
    who: Lenajava1
    splitFrom: KB-C923D0DB
---
The confirmation email's link points to /account/confirmemail with the user id and an identity token; opened in a fresh browser with no session it renders the Email confirmation success page asking the user to sign in, and does not sign them in. Opening a freshly resent link for an already-confirmed account shows the same success page rather than an error.
