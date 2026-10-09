---
id: KB-B9FD04A3
subject: /forgot-password sends Mutation sendPasswordResetEmail and shows 'We sent you a reset password link to your inbox'; a ResetPasswordEmailNotification is journaled
plane: experiential
question: What does the storefront forgot-password form call and show for a registered email?
status: active
appliesTo:
  - axis: feature
    value: password-reset
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /forgot-password
  - coordinate: Mutation.sendPasswordResetEmail
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:56:17.157Z
    by: session:df9d131f
    who: Lenajava1
---
On theme vc-frontend-next 3.0.0-alpha.2685 (2026-10-09), /forgot-password ('Reset your password', Submit disabled until an email is entered) submitted for a registered B2B-store account sends GraphQL mutation sendPasswordResetEmail(command:{storeId, cultureName, loginOrEmail, urlSuffix:'/reset-password'}) which returned data true with no errors; the page replaced the form with 'We sent you a reset password link to your inbox' and a Home page link. Within a second POST /api/notifications/journal (keyword = the email) listed a ResetPasswordEmailNotification, subject 'Reset password link', status Sent.
