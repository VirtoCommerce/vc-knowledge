---
id: KB-ED01C2A4
subject: A storefront token without organization_id takes the contact's current (last-used) organization; organization_id picks the membership, organizationId is ignored
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
    resolved: claim-amended
    resolvedAt: 2026-10-08T11:38:05.775Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/storefront-batch-20261008
    resolution: Without organization_id the token takes the first stored organization role, so a maintainer keeps the full set - live on vcst_qa and vcptcore_stable 2026-10-08 (vcptcore_dev not reachable). Subject and body corrected.
    amendedFrom: 31606bfc5c74
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; theme=2.59.0; module:VirtoCommerce.Xapi=3.1026.0; module:VirtoCommerce.XCart=3.1039.0; module:VirtoCommerce.Marketing=3.1009.0; module:VirtoCommerce.MarketingExperienceApi=3.1004.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1020.0; module:VirtoCommerce.Tax=3.1007.0; store=B2B-store; setting:Stores.TaxCalculationEnabled=true
    at: 2026-10-08T11:37:56.516Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: organization_id and organizationId scope the token to that membership (11 vs 2 permissions); without it, the token takes the role of the first stored organization of the contact: single-org maintainer 11, single-org employee 2, two-org user the first stored org role; global vs membership role made no difference."
    investigation: true
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; theme=2.51.2; module:VirtoCommerce.Xapi=3.1012.3; module:VirtoCommerce.XCart=3.1021.2; module:VirtoCommerce.Marketing=3.1005.0; module:VirtoCommerce.Customer=3.1011.1; module:VirtoCommerce.ProfileExperienceApiModule=3.1009.1; module:VirtoCommerce.Tax=3.1003.0; store=B2B-store; setting:Stores.TaxCalculationEnabled=true
    at: 2026-10-08T11:37:57.320Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: same rules; maintainer set has 8 permissions here."
    investigation: true
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:37:34.811Z
    by: session:62b7421e
    who: Lenajava1
    contradicts: true
    note: "vcst_qa 2026-10-08: a token WITHOUT organization_id is scoped to an organization the platform picks and carries THAT membership's permissions, not a fixed two-permission view set. A member whose only role grants xapi:my_organization:return:view got exactly [return:view] (1 permission); an org-employee member got the 2 view permissions only because that is what org-employee holds. For a contact in two organizations the picked organization changed between two seeds (once the first, once the second), so omitting organization_id is non-deterministic, not view-only."
    resolved: claim-amended
    resolvedAt: 2026-10-12T18:22:00.180Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "The dispute was right that it is not the first stored organization: an unscoped token takes CurrentOrganizationId, the last organization_id the contact signed in with, which explains its two differing runs; camelCase organizationId is ignored (vcst, 2026-10-12, source cited)."
    amendedFrom: 7db46761cff1
  - method: observation
    deployment: vcst
    conditions: platform=3.1078.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1021.0; module:VirtoCommerce.Xapi=3.1027.0; store=B2B-store
    at: 2026-10-12T18:21:58.159Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-12, 12-organization contact: unscoped password-grant mints -> the contact's currentOrganizationId (stored index 5, not the first) 3/3; organization_id=X -> X (12/12) and X becomes currentOrganizationId, after which unscoped mints return X 3/3; camelCase organizationId ignored; a non-member organization_id -> first accessible org, persisted. Source: vc-module-customer 3.1029.0 Constants.cs L5, OrganizationIdClaimProvider L112-139, OrganizationAccessResolver L27-39, CurrentOrganizationIdTokenRequestHandler L22-43. Contact restored to its original current org."
    investigation: true
    bodyHash: 3170d75bb5b5
---
Passing organization_id together with storeId on the storefront password grant scopes the token to that organization, and me.permissions returns that membership's role: one user who was maintainer of A and employee of B got 11 permissions for A and 2 (storefront:organization:view, storefront:user:view) for B on vcst_qa, 8 and 2 on vcptcore_stable (2026-10-08). Only the snake_case parameter is read (vc-module-customer OpenIddict Constants.Parameters.OrganizationId = "organization_id"): on vcst on 2026-10-12 a camelCase organizationId was ignored and the token kept the current organization. An organization_id the contact is not a member of is silently replaced on the password grant by the first accessible organization (OrganizationIdRequestValidator).

Every token request that carries organization_id is persisted as the member's CurrentOrganizationId (CurrentOrganizationIdTokenRequestHandler; the storefront resends the last-used organization at sign-in). Omitting organization_id does NOT reduce a token to the two view permissions and is not random: the token takes CurrentOrganizationId if the contact can still access it, else DefaultOrganizationId, else the first accessible organization in the contact's organizations as stored (OrganizationAccessResolver), and me.contact.organizationId names it. Observed on vcst 2026-10-12 with a 12-organization contact: three unscoped mints returned the current organization (stored index 5, not the first) 3/3; after a mint with organization_id=X, unscoped mints returned X. So the organization an unscoped token gets changes with the contact's last scoped sign-in, which is why two runs on one contact can disagree.
