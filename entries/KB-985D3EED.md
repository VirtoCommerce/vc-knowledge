---
id: KB-985D3EED
subject: The storefront MFA widgets register only when the platform lists VirtoCommerce.MFA with a version that has the mfa xAPI schema (3.1000.2+ on the 3.1000 line, 3.1002.0+); the organization widget also needs xapi:my_organization:mfa:manage, which no stock organization role carries
plane: experiential
question: When does the storefront show the MFA widget on Account Security and Company info, and which role grants xapi:my_organization:mfa:manage?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/info
  - coordinate: /account/security
  - coordinate: Query.mfaSettings
evidence:
  - method: source
    deployment: vc-frontend@2.62.0-pr-2622-caac275a
    at: 2026-11-04T11:18:03.179Z
    by: session:325c07df
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; theme=2.62.0-pr-2622-caac-caac275a
    at: 2026-11-04T14:24:47.401Z
    by: session:7f21950d
    bodyHash: cdad43e60dfd
    who: Lenajava1
    note: "Observed live 2026-11-04 (store windows CustomerChoice / AdminManaged): a member whose run org role adds xapi:my_organization:mfa:manage (present in the storefront me permissions) sees the company widget on /company/info; a member without it sees none, also while impersonated."
---
Source read of vc-frontend PR 2622 (client-app/modules/mfa/index.ts, constants.ts, components/mfa-settings-widget.vue) and vc-module-mfa PR 3: init() returns without registering anything unless useModules() lists VirtoCommerce.MFA with a version that hasMfaSchema (SCHEMA_RELEASES 3.1000.2 and 3.1002.0; the 3.1001 line never got the schema); the module list needs setting XAPI.Security.ReturnModuleVersion. The user widget renders when storeMode is AdminManaged or CustomerChoice or isRequired is true; the organization widget renders only in those two store modes and only when the user holds xapi:my_organization:mfa:manage. The permission is registered by the MFA module (group XAPI) and the README says to add it to an organization role. Live 2026-11-04 on vcst_qa: the roles Organization maintainer, Organization manager, Organization employee and Purchasing agent do not contain it, and no store is in AdminManaged or CustomerChoice, so no storefront user can see the company widget.
