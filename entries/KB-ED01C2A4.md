---
id: KB-ED01C2A4
subject: a storefront token without organization_id carries only view permissions
plane: experiential
question: How do I get an organization-scoped storefront token from /connect/token so me.permissions reflects that organization's role?
questions:
  - text: Why does my company account say I lack permission to edit the organization right after signing in?
  - text: How do I request a storefront token scoped to one of a user's organizations?
  - text: Which permissions does me return when the token request omits organization_id?
  - text: Does the organization_id parameter on the token endpoint work in camelCase as well?
  - text: Why do organization mutations fail with a missing my_organization edit permission for a valid user?
concepts:
  - id: access-token
  - id: organization-role
  - id: permission
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /connect/token
  - coordinate: Query.me
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:30.039Z
    by: session:memimpor
    who: Lenajava1
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:53:15.335Z
    by: session:b6bd53fc
    contradicts: true
    note: "On vcptcore_dev (2026-10-06) tokens minted WITHOUT organization_id for an Organization maintainer, an Organization manager and another organization's maintainer carried 11, 10 and 12 permissions including xapi:my_organization:edit / order:view / user:invite — not only the two view permissions. Organization-scoped tokens behaved as the entry says (a two-organization user got 12 vs 2). Not separated: whether these accounts hold the role globally or per membership, which may explain the difference."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:37:34.811Z
    by: session:62b7421e
    who: Lenajava1
    contradicts: true
    note: "vcst_qa 2026-10-08: a token WITHOUT organization_id is scoped to an organization the platform picks and carries THAT membership's permissions, not a fixed two-permission view set. A member whose only role grants xapi:my_organization:return:view got exactly [return:view] (1 permission); an org-employee member got the 2 view permissions only because that is what org-employee holds. For a contact in two organizations the picked organization changed between two seeds (once the first, once the second), so omitting organization_id is non-deterministic, not view-only."
---
Passing organization_id together with storeId on the storefront token request scopes the token to that organization, and me.permissions then returns that membership's role permissions (a maintainer and an employee of two organizations got different sets). On the current platform build organization_id works in either casing; an older claim that camelCase was ignored no longer holds. Omitting it entirely still mints a token (200) but with only two permissions, storefront:organization:view and storefront:user:view, so organization-scoped mutations fail with a missing xapi:my_organization:edit permission that reads like a broken account.
