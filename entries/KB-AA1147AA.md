---
id: KB-AA1147AA
subject: Organization.status is the organization's own status, not the caller's membership status
plane: experiential
question: Which field on Organization tells the caller's membership status and lock - status, myStatusInOrganization or isLockedForCurrentUser?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Organization.status
  - coordinate: Organization.myStatusInOrganization
  - coordinate: Organization.isLockedForCurrentUser
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:18:15.323Z
    by: session:memimpor
    who: Lenajava1
---
On the xAPI Organization type, status is the organization's own member status (it can be null for one organization and Approved for another, independent of any membership). myStatusInOrganization is the caller's membership status in that organization and stayed Approved even for a locked membership. isLockedForCurrentUser is the caller's lock state per organization and reads true on the locked one. Contact isLockedInOrganization reflects only the current active organization, so it is no signal for a lock in a sibling organization.
