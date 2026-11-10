---
id: KB-E61A40EE
subject: Org-scoped token permissions come from organization and membership role IDs looked up by FindByIdAsync, and an ID that is not a role is skipped without an error
plane: experiential
question: Why does an organization member's /connect/token lack the permissions of a membership role that the membership record shows?
questions:
  - text: Does MembershipRolesWhitelist filter the permissions in an organization-scoped storefront token?
  - text: What happens when an organization membership role id does not match any platform role id?
  - text: Why does an Organization maintainer member only get Organization employee permissions in the token?
status: active
appliesTo:
  - axis: grant
    value: password
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/customer/organization-memberships/search
evidence:
  - method: source
    deployment: vc-module-customer@3.1029.1
    at: 2026-11-10T07:55:25.163Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
    note: "distinct: The related entries record what was observed: an organization-level role is inherited, a token picks its organization, and a maintainer's permissions were missing. This entry is the source mechanism: role IDs are resolved by ID only, an ID that matches no role is dropped silently, and the whitelist plays no part in building the token."
---
In vc-module-customer 3.1029.1, OrganizationIdClaimProvider (Data/OpenIddict) builds the permission claims of an org-scoped token from the union of the organization-level roles and the membership roles, de-duplicated by RoleId (OrganizationMembershipSearchService.MergeRoles). It resolves each RoleId with RoleManager.FindByIdAsync. When the result is null it skips that role: nothing is logged and no error is returned. The lookup is by ID only, not by name. The membership record keeps the roleName as it was written, so the Admin UI still shows the role. MembershipRolesWhitelist is not read anywhere on this path; it limits only which roles can be invited or assigned. The same provider is unchanged in 3.1033.0. Before treating missing org permissions as a bug, check that each membership roleId resolves through the platform roles API.
