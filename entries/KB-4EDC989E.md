---
id: KB-4EDC989E
subject: xAPI resolves a contact's current organization from contact.organizations, not from the OrganizationMembership entity
plane: experiential
question: Why does Query.me return contact.organizationId null for a user who has an Approved OrganizationMembership?
questions:
  - text: Why does a user with an approved company membership show no company on the storefront?
  - text: Query.me contact.organizationId null Approved OrganizationMembership
  - text: Does xAPI read the current organization from contact.organizations or OrganizationMembership?
  - text: What happens to organizationId for a locked membership with the organization in contact.organizations?
  - text: Why do organization-mode loyalty balances fall back to personal scope for a seeded member?
concepts:
  - id: organization-membership
  - id: organization
  - id: contact
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: xapi
  - axis: surface
    value: rest
anchors:
  - coordinate: Query.me
  - coordinate: GET /api/members/{id}
  - coordinate: PUT /api/members
  - coordinate: POST /api/customer/organization-memberships/search
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-01T08:48:19.161Z
    by: session:4b2eac5d
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-10-05T07:19:20.066Z
    by: session:51fcfb97
    who: kutasinaelena
    note: "seed-edge-cases multi-org: 11 Approved memberships + empty contact.organizations -> after PUT /api/members adding the 11 ids, Query.me returned organizationId + organizations.totalCount=11"
---
On a Customer module that has the OrganizationMembership entity (POST /api/customer/organization-memberships/search), a storefront user with an Approved, unlocked membership to an organization still got me.contact.organizationId = null and an empty me.contact.organizations list, because the contact's own organizations array (GET /api/members/{contactId}) was empty. A working organization account had the organization in both places. Adding the organization id to contact.organizations with PUT /api/members made xAPI resolve it as the current organization immediately. A locked membership (isLocked=true) with the organization in contact.organizations lists the organization but leaves organizationId null. Consequence: a seeder or test that creates only the membership leaves the user organization-less on the storefront, and org-scoped features (for example organization-mode loyalty balances, which read the ambient session organization) silently fall back to the personal scope.
