---
id: KB-ED01C2A4
subject: a storefront token without organization_id carries only view permissions
plane: experiential
question: How do I get an organization-scoped storefront token from /connect/token so me.permissions reflects that organization's role?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /connect/token
  - coordinate: Query.me
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:16:30.039Z
    by: session:memimpor
    who: Lenajava1
---
Passing organization_id together with storeId on the storefront token request scopes the token to that organization, and me.permissions then returns that membership's role permissions (a maintainer and an employee of two organizations got different sets). On the current platform build organization_id works in either casing; an older claim that camelCase was ignored no longer holds. Omitting it entirely still mints a token (200) but with only two permissions, storefront:organization:view and storefront:user:view, so organization-scoped mutations fail with a missing xapi:my_organization:edit permission that reads like a broken account.
