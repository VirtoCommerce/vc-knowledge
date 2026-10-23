---
id: KB-B8EDAD02
subject: An OrganizationMembership's status round-trips through a full-row PUT, and lock/unlock act on it by membership id; contact currentOrganizationId is set through PUT /api/members
plane: experiential
question: How do I set one organization membership of a multi-org contact to Invited/Rejected/Deleted or locked, and pin the contact's current organization, through REST?
questions:
  - text: Which REST call changes an organization membership status?
  - text: How do I lock a single organization membership of a contact?
  - text: How do I set a contact's current organization via the admin API?
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/customer/organization-memberships/{id}/lock
  - coordinate: PUT /api/customer/organization-memberships/{id}
  - coordinate: PUT /api/members
evidence:
  - method: observation
    deployment: vcst_qa
    build: d70745c02c9df3fe
    conditions: module:VirtoCommerce.Customer=3.1030.0
    at: 2026-10-23T11:39:43.414Z
    by: session:7ed484a4
    who: kutasinaelena
---
On vcst_qa 2026-10-23 (Customer 3.1030.0), for a fresh AGENT-TEST contact created with organizations=[A,B] and one membership row per org (POST /api/customer/organization-memberships with an org-scoped role): POST /api/customer/organization-memberships/{id}/lock with {lockoutEnd:null} read back isLocked=true, POST .../{id}/unlock read back isLocked=false; GET the row then PUT /api/customer/organization-memberships/{id} with the whole row and status=Invited read back Invited, and the same PUT with Approved restored it. GET /api/members/{contactId}, set currentOrganizationId, PUT /api/members read back the new currentOrganizationId. The other membership was untouched.
