---
id: KB-DA14E8B7
subject: block on an un-accepted invitation is irreversible
plane: experiential
question: What happens to a pending organization invitation if the maintainer blocks and then unblocks that row?
status: active
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
    at: 2026-09-12T08:24:16.232Z
---

On the B2B storefront roster, Block user / Unblock user write the CONTACT's status field, and an outstanding invitation lives in that same field - so the pair is a one-way door that silently ends the invitation. Block (lockOrganizationContact) overwrites contact.status Invited -> Locked and the row stops showing the 'Invite sent' placeholder name, so it no longer reads as an invitation at all. Unblock (unlockOrganizationContact) does not restore what was there: it writes contact.status = Approved and additionally resets the security account's lockoutEnd from the invitation's 9999-12-31 sentinel to 0001-01-01. The account still has passwordHash null and emailConfirmed false - nobody registered - yet the roster now reports the person as Active, which is the roster asserting something the platform does not hold. Nothing on either surface can put the row back to Invited, and there is no way to start over: Delete only detaches the contact from the organization, the account survives holding the address, and re-inviting that address is refused as a duplicate. So treat Block on an un-accepted invitee as irreversible destruction of the invitation - read the row's status before opening the menu, because the menu looks identical for an invitee and for a colleague who has worked there for a year.
