---
id: KB-205EAEF9
subject: "Invite member emails textarea: aria-invalid=\"true\" + aria-describedby to <textarea-id>-details while the validation message shows; both attributes absent once the value is valid"
plane: experiential
question: Is the /company/members Invite member emails textarea error exposed with aria-invalid and aria-describedby?
questions:
  - text: Which element does the invite emails textarea's aria-describedby point at when an email is invalid?
  - text: Does the invite textarea keep aria-invalid after entering a valid email?
status: active
appliesTo:
  - axis: role
    value: organization-maintainer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/members
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: theme=2.62.0-pr-2629-8fcc-8fccd4be; browser=chromium
    at: 2026-11-09T13:01:48.296Z
    by: session:ff42a502
    who: Lenajava1
---
Theme 2.62.0-pr-2629 (VCST-5994), Chromium 1440, org maintainer, /company/members > Invite members. On open the emails textarea (native required) has no aria-invalid and no aria-describedby; its <id>-details div exists empty. Typing "not-an-email" + Tab: aria-invalid="true", aria-describedby="textarea-<n>-details", text "Incorrect email addresses: not-an-email". Clearing + Tab: same pair, text "This field is required". A valid address + Tab: both attributes absent. The accessible name stays the label "Enter email addresses of people you want to invite" (no asterisk, not the error text). Send stays disabled throughout because Role is disabled (no roles on this stand).
