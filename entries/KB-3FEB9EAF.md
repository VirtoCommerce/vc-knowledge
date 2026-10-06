---
id: KB-3FEB9EAF
subject: A confirmation email is resent only from the Admin user blade, not from the storefront sign-in page
plane: experiential
question: How is a confirmation email resent?
questions:
  - text: I never got my verification email, can I request a new one from the sign-in page?
  - text: How does a back-office admin resend an account's email confirmation?
  - text: Where can QA check that a resent confirmation email was actually generated?
  - text: Is the missing resend link on the storefront sign-in page a defect or by design?
concepts:
  - id: email-confirmation
  - id: notification
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:17:26.163Z
    by: session:memimpor
    who: Lenajava1
    splitFrom: KB-C923D0DB
---
The storefront /sign-in page has no self-service resend link, BY DESIGN; the resend path is the Admin user blade's Resend link, which produces an Email confirmation notification visible in the Admin notification activity feed with a preview of the body.
