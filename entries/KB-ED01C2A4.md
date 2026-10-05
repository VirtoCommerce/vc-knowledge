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
---
Passing organization_id together with storeId on the storefront token request scopes the token to that organization, and me.permissions then returns that membership's role permissions (a maintainer and an employee of two organizations got different sets). On the current platform build organization_id works in either casing; an older claim that camelCase was ignored no longer holds. Omitting it entirely still mints a token (200) but with only two permissions, storefront:organization:view and storefront:user:view, so organization-scoped mutations fail with a missing xapi:my_organization:edit permission that reads like a broken account.
