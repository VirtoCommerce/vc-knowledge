---
id: KB-DC65E5F8
subject: blocking a pending invitation destroys the invitation
plane: experiential
question: What happens to a pending organization invitation if the maintainer blocks and then unblocks that row?
status: retired
supersededBy: KB-DA14E8B7
refutableBy: observation
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: storefront-xapi
  - axis: principal
    value: org-maintainer
anchors:
  - coordinate: GET /company/members
  - coordinate: Mutations.lockOrganizationContact
  - coordinate: Mutations.unlockOrganizationContact
  - coordinate: ContactType.status
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:17:17.111Z
---

On the B2B storefront roster, Block user / Unblock user write the CONTACT's status field, and an outstanding invitation lives in that same field - so the pair is a one-way door that silently ends the invitation. Block (lockOrganizationContact) overwrites contact.status Invited -> Locked and the row stops showing the 'Invite sent' placeholder name, so it no longer reads as an invitation at all. Unblock (unlockOrganizationContact) does not restore what was there: it writes contact.status = Approved and additionally resets the security account's lockoutEnd from the invitation's 9999-12-31 sentinel to 0001-01-01. The account still has passwordHash null and emailConfirmed false - nobody registered - yet the roster now reports the person as Active, and there is no storefront control that can put the row back to Invited. Treat block/unblock on an un-accepted invitee as destructive: read the row's status before using it, and if you need the person back, delete the row and invite again rather than trying to undo.

**Retired.** Its closing advice was wrong and I proved it wrong twenty minutes later: 'delete the row and invite again' does not work, because Delete leaves the security account alive and the address stays taken, so the re-invite is refused with DuplicateEmail. Everything else in it held; this replacement keeps the mechanism and drops the bad remedy. Superseded by KB-DA14E8B7.
