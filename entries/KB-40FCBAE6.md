---
id: KB-40FCBAE6
subject: a failed notification after a successful share save shows a warning and offers no resend
plane: experiential
question: what happens in the list Share dialog when the notification send fails after the save
questions:
  - text: I shared my list but the email didn't go out - can I send it again?
  - text: Is a shared list still saved when the recipient notification fails?
  - text: What toast does the Share dialog show when the save succeeds but the message send fails?
  - text: Does the list Share dialog offer any way to re-notify a recipient after a failed notification?
concepts:
  - id: list-sharing
  - id: customer-communication
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists
  - coordinate: Mutation.sendCustomerCommunication
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:54:37.063Z
    by: session:p3784
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:54:35.679Z
    by: session:p56416
    who: Lenajava1
---
On the vc-frontend#2476 dialog, when SendCustomerCommunication fails after ChangeWishlist succeeded, the list stays saved, the dialog closes and a warning toast reads 'The list was saved, but the notification could not be sent.'; on reopen there is no way to re-notify that recipient.
