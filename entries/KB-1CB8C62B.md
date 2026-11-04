---
id: KB-1CB8C62B
subject: In a Disabled store a customer whose organization role exists only on the organization membership (no user-level role) reads isDisabledModeSkipped=false, like a role-less customer; only roles in ApplicationUser.Roles make it a Backoffice account
plane: experiential
question: Does an organization role held only on the organization membership make a customer count as Backoffice in a Disabled store (isDisabledModeSkipped)?
questions:
  - text: "Which org-role customers lose the Disabled-store skip: membership-scoped roles or user-level roles?"
  - text: isDisabledModeSkipped for an Organization employee created through /api/customer/organization-memberships
status: active
appliesTo:
  - axis: feature
    value: mfa
  - axis: store-mode
    value: disabled
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/mfa/modes/users/{id}/status
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.Enabled=false; setting:MultiFactorAuthentication.StoreMode=Disabled (run store)
    at: 2026-11-04T14:14:52.160Z
    by: session:7f21950d
    who: Lenajava1
    note: "distinct: KB-9CE07DBB observed B2B-store customers whose org role sits in user.Roles (true); this is the membership-scoped org role, which reads false — the role's location decides the classification."
---
vcst 2026-11-04, disposable store in StoreMode Disabled, global MFA off, GET /api/mfa/modes/users/{id}/status row for that store: own customer with no roles -> isDisabledModeSkipped false; own customer whose only org role is org-employee on its organization membership (user.Roles empty, created via POST /api/customer/organization-memberships) -> false; own customer holding a back-office role with platform:security:read in user.Roles -> true; Manager with no store -> true. So the Backoffice classification (IAdminUIAccessPolicy over user.Roles permissions) only sees user-level roles; the org-role customers KB-9CE07DBB found true carry the role in user.Roles. Source: MfaRequirementService.CanSkipCodeInDisabledStoreAsync / IsBackofficeAccountAsync collect the permissions of user.Roles.
