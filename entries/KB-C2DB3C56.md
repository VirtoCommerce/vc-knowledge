---
id: KB-C2DB3C56
subject: User and organization MFA mode changes leave no platform changelog entry; a store MFA mode change is logged as Store Modified with no detail
plane: experiential
question: Is an MFA mode change (PUT api/mfa/modes/users/{id}, updateMfaUserMode) recorded in POST /api/platform/changelog/v2/search or the user's Changes blade?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/changelog/v2/search
  - coordinate: PUT /api/mfa/modes/users/{id}
  - coordinate: PUT /api/mfa/modes/organizations/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f
    at: 2026-11-04T14:01:59.107Z
    by: session:7f21950d
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f
    at: 2026-11-04T14:13:40.565Z
    by: session:7f21950d
    bodyHash: 84c607b64036
    who: Lenajava1
    note: "Re-observed via API only 2026-11-04: after ~24 user/org mode changes (admin PUT and xAPI mutations) changelog v2 search from the run start held only Added entries for those ids and Store Modified entries for store setting writes; no Modified entry for any mode change. The Changes blade was not opened in this run."
---
After changing user and organization MFA modes through PUT /api/mfa/modes/users/{id}, PUT /api/mfa/modes/organizations/{id} and the storefront mutations, POST /api/platform/changelog/v2/search returned no entry for them, and the user's Changes blade in the Admin Shell showed only Added/Created. A change of the store setting MultiFactorAuthentication.StoreMode was logged as Store Modified with detail null. The sign-in log records RequiresTwoFactor and MFA sign-ins, not mode changes. There is no audit trail of who turned MFA on or off for a user or a company.
