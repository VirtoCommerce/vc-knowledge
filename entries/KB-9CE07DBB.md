---
id: KB-9CE07DBB
subject: In a Disabled store the status API marks Managers and customers holding an organization role as isDisabledModeSkipped=true; only role-less customers of that store are lenient
plane: experiential
question: Which users of a StoreMode=Disabled store does GET /api/mfa/modes/users/{id}/status report as still following the global MFA setting (isDisabledModeSkipped)?
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
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.Enabled=false; setting:MultiFactorAuthentication.StoreMode=Disabled (B2B-store)
    at: 2026-11-04T12:04:24.124Z
    by: session:325c07df
    who: Lenajava1
---
2026-11-04, read-only, admin token, store B2B-store has StoreMode=Disabled. GET /api/mfa/modes/users/{id}/status row for B2B-store, users whose storeId is B2B-store: Customer with no roles (14 of 14) isDisabledModeSkipped=false (own customer, the code is skipped); Customer or untyped user with the role Organization employee or Purchasing agent (35 of 35) isDisabledModeSkipped=true; Manager type (11 of 11) isDisabledModeSkipped=true, with or without roles. A customer of another store (TestStore) also reads true. Both roles carry permissions (storefront:organization:view, storefront:user:view), and README treats any user with a permission as a Backoffice account through IAdminUIAccessPolicy, so a B2B storefront user with an org role does not skip the code in a Disabled store. Same evaluator the token endpoint uses per README; the token endpoint itself was not run (global MFA on is a shared write).
