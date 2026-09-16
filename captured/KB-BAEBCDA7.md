---
id: KB-BAEBCDA7
subject: an organization invitation cannot be cancelled or resent
plane: experiential
question: How does an invited person complete registration into a B2B organization, and can a maintainer resend or cancel the invitation?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: admin-ui
  - axis: principal
    value: org-maintainer
anchors:
  - coordinate: GET /company/members
  - coordinate: Mutations.inviteUser
  - coordinate: RegistrationInvitationEmailNotification
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:23:37.378Z
    by: session:f00f5968
---

There is no cancel control and no resend control for an outstanding organization invitation, on either surface. The storefront roster's row menu for an Invited row is the same three entries as for any member - Edit role, Block user, Delete - and Admin's only invitation affordance is the Companies-and-contacts 'Invite customers' blade, which sends a NEW invitation and has no notion of an existing one. The three near-misses all fail: (1) Re-inviting the same address is refused - inviteUser returns succeeded:false with DuplicateUserName AND DuplicateEmail, because the invitation already provisioned a real account holding that address, so there is no resend-by-repeat. The dialog surfaces only the DuplicateEmail message and swallows the other. (2) Delete does not free the address either: it removes the organization membership only, so the account survives and the address stays taken forever - after Delete the same address still fails to invite. A maintainer therefore cannot undo an invitation at all, only hide it from the roster. (3) The account blade's 'Resend link' beside Verified resends the email-confirmation link, not the invitation. Completion is the invited person's act and needs the emailed link: inviting really does send a RegistrationInvitationEmailNotification, which the Admin Notification activity feed records with its own status, so on a stand where nobody can read the mailbox that feed is where you check whether the mail left at all - and its 'Success' means handed to the transport, not delivered. Plan accordingly: an invitation to a wrong address is not recoverable through either UI; fixing it needs an administrator to delete the account under Security > Users and the contact under Contacts.
