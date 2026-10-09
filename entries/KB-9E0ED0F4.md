---
id: KB-9E0ED0F4
subject: Query.organizationReturns lists an organization's returns only for an active member holding xapi:my_organization:return:view; everyone else gets one uniform Forbidden
plane: experiential
question: Who can list an organization's returns with organizationReturns, and what do others get?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.organizationReturns
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:53:16.950Z
    by: session:b6bd53fc
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T14:50:56.055Z
    by: session:62b7421e
    who: Lenajava1
    note: "Return 3.1005.0-pr-28-62f9: a maintainer-type org member whose token held only xapi:my_organization:edit/order:view/user:invite got the uniform Forbidden for their own organization, an empty organizationId and an unknown one (data null); anonymous got Unauthorized; return(id:\"unknown\") returned null with no error. A scan of every role (GET security/roles/{id}) found none holding xapi:my_organization:return:view."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T16:00:49.613Z
    by: session:62b7421e
    who: Lenajava1
    note: "Fresh: non-holder (own org, empty id, random GUID, other org), a non-holder in the same org and a non-member administrator all got the uniform Forbidden with data null; Invited/Rejected/Deleted/locked memberships refused on a token issued before the block. A multi-org contact whose token was scoped to the org WITHOUT the permission was answered for the org WITH it."
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:24:13.375Z
    by: session:61a7b05a
    who: yuskithedeveloper
    note: "Return 3.1005.0-pr-28-fb4f, 2026-10-09: same uniform Forbidden (data null) for a contact holding the permission through a global role but only ASSOCIATED with the organization (unscoped and organization-scoped token), for a contact with an Approved membership row but without the organization in its own organizations list, and for a member with no membership row whose contact status is Rejected or Invited (old and fresh token). The same member with contact status null or Locked was answered."
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:33:54.969Z
    by: session:61a7b05a
    who: yuskithedeveloper
    note: "Return 3.1005.0-pr-28-fb4f, lane B 2026-10-09: an employee without the permission (organization-scoped and unscoped token) and another organization's holder both got errors[] Forbidden \"The organization's returns are not available to this contact.\" with data null; anonymous got Unauthorized for organizationReturns and for return(id)."
---
On Return 3.1005.0-pr-28-62f9, organizationReturns(storeId, organizationId) answered for a member of that organization whose roles include xapi:my_organization:return:view, whether granted through a global role, the membership role or a role on the organization itself. A member without it (purchasing agent, employee, an Organization manager holding the other xapi:my_organization grants), a holder from another organization, an administrator who is not a member, an empty or unknown organizationId all got errors[] Forbidden "The organization's returns are not available to this contact." with data null, never an empty list; anonymous got Unauthorized. The requested organizationId decides, whatever organization the token is scoped to. A locked, Invited or Rejected membership is refused; a locked account gets UserLocked on every return operation. On this deployment no role held the permission until an admin granted it.
