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
---
On Return 3.1005.0-pr-28-62f9, organizationReturns(storeId, organizationId) answered for a member of that organization whose roles include xapi:my_organization:return:view, whether granted through a global role, the membership role or a role on the organization itself. A member without it (purchasing agent, employee, an Organization manager holding the other xapi:my_organization grants), a holder from another organization, an administrator who is not a member, an empty or unknown organizationId all got errors[] Forbidden "The organization's returns are not available to this contact." with data null, never an empty list; anonymous got Unauthorized. The requested organizationId decides, whatever organization the token is scoped to. A locked, Invited or Rejected membership is refused; a locked account gets UserLocked on every return operation. On this deployment no role held the permission until an admin granted it.
