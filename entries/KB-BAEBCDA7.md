---
id: KB-BAEBCDA7
subject: an organization invitation cannot be cancelled or resent
plane: experiential
question: How does an invited person complete registration into a B2B organization, and can a maintainer resend or cancel the invitation?
questions:
  - text: I invited a colleague to the wrong email address - can I cancel or resend that invitation?
  - text: Why does inviting the same address again fail with a duplicate user name and email error?
  - text: Does deleting an invited member from the company roster free the email address for a new invitation?
  - text: How can an admin confirm the invitation email actually left the system when nobody can read the mailbox?
  - text: Does the Resend link on the account blade resend the organization invitation or the email confirmation?
  - text: Does inviteUser provision a real account immediately, before the invited person completes registration?
concepts:
  - id: organization-invitation
  - id: member-deletion
  - id: notification
status: active
appliesTo:
  - axis: principal
    value: org-maintainer
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: admin-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: GET /company/members
  - coordinate: Mutations.inviteUser
  - coordinate: RegistrationInvitationEmailNotification
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:23:37.378Z
    by: session:f00f5968
    build: be10ad6a04a12ec3
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 6feb83b80e6f1f12
    conditions: theme=2.63.0-alpha.2681; store=B2B-store; role=Organization maintainer
    at: 2026-11-26T09:57:32.957Z
    by: session:779b94fc
    bodyHash: 9c274eef6479
    contradicts: true
    contested: virtocommerce.fileexperienceapi 3.1010.0 vs 3.1000.0; virtocommerce.notifications 3.1021.0 vs 3.1001.8; virtocommerce.profileexperienceapimodule 3.1028.0 vs 3.1000.2; virtocommerce.xapi 3.1033.0 vs 3.1003.2; virtocommerce.xcart 3.1048.0 vs 3.1000.6; virtocommerce.xcatalog 3.1027.0 vs 3.1000.5; …
    note: 2026-11-26 on vcst_qa the /company/members row menu for an Invited (not yet registered) row offers Resend invite / Revoke invite / Login on behalf, not Edit role/Block/Delete. Resend invite shows toast 'Invite resent'; Revoke invite opens a 'Revoke invite' confirmation (Cancel/OK) and OK shows toast 'Invite revoked', after which the TechFlow membership is at status Deleted and the row stays in the roster with an Inactive badge. So on this build an outstanding invitation can be resent and revoked from the storefront.
---

There is no cancel control and no resend control for an outstanding organization invitation, on either surface. The storefront roster's row menu for an Invited row is the same three entries as for any member - Edit role, Block user, Delete - and Admin's only invitation affordance is the Companies-and-contacts 'Invite customers' blade, which sends a NEW invitation and has no notion of an existing one. The three near-misses all fail: (1) Re-inviting the same address is refused - inviteUser returns succeeded:false with DuplicateUserName AND DuplicateEmail, because the invitation already provisioned a real account holding that address, so there is no resend-by-repeat. The dialog surfaces only the DuplicateEmail message and swallows the other. (2) Delete does not free the address either: it removes the organization membership only, so the account survives and the address stays taken forever - after Delete the same address still fails to invite. A maintainer therefore cannot undo an invitation at all, only hide it from the roster. (3) The account blade's 'Resend link' beside Verified resends the email-confirmation link, not the invitation. Completion is the invited person's act and needs the emailed link: inviting really does send a RegistrationInvitationEmailNotification, which the Admin Notification activity feed records with its own status, so on a stand where nobody can read the mailbox that feed is where you check whether the mail left at all - and its 'Success' means handed to the transport, not delivered. Plan accordingly: an invitation to a wrong address is not recoverable through either UI; fixing it needs an administrator to delete the account under Security > Users and the contact under Contacts.
