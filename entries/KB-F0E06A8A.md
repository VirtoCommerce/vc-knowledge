---
id: KB-F0E06A8A
subject: Revoke invite on /company/members sets the invitee's organization membership to Deleted and the row stays in the roster with an Inactive badge
plane: experiential
question: After an organization maintainer revokes a pending invite on /company/members, does the invited row disappear?
questions:
  - text: What status does revokeOrganizationInvite leave on the invitee's organization membership?
  - text: Why does a revoked invite still show in the company members list as Inactive?
status: active
appliesTo:
  - axis: principal
    value: org-maintainer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/members
  - coordinate: Mutations.revokeOrganizationInvite
  - coordinate: POST /api/customer/organization-memberships/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: 6feb83b80e6f1f12
    conditions: theme=2.63.0-alpha.2681; store=B2B-store; role=Organization maintainer
    at: 2026-11-26T09:57:39.461Z
    by: session:779b94fc
---
2026-11-26 on vcst_qa (theme 2.63.0-alpha.2681, B2B-store), as TechFlow Organization maintainer: invited a new address (row badge Invited), Actions > Revoke invite > OK showed toast 'Invite revoked'. The row did not leave the roster: it stays (also after a page reload) labelled 'Invite sent' with the role and an 'Inactive' badge. Admin POST /api/customer/organization-memberships/search for the invitee's user returns the TechFlow membership with status 'Deleted' - the membership row is kept, not removed, and the platform account the invite created still exists. Same visible outcome as Delete on an active member.
