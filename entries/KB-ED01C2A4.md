---
id: KB-ED01C2A4
subject: A storefront token without organization_id takes the role of the contact's first stored organization; organization_id (either casing) picks the membership
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
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; theme=2.59.0; module:VirtoCommerce.Xapi=3.1026.0; module:VirtoCommerce.XCart=3.1039.0; module:VirtoCommerce.Marketing=3.1009.0; module:VirtoCommerce.MarketingExperienceApi=3.1004.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1020.0; module:VirtoCommerce.Tax=3.1007.0; store=B2B-store; setting:Stores.TaxCalculationEnabled=true
    at: 2026-10-08T11:37:56.516Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: organization_id and organizationId scope the token to that membership (11 vs 2 permissions); without it, the token takes the role of the first stored organization of the contact: single-org maintainer 11, single-org employee 2, two-org user the first stored org role; global vs membership role made no difference."
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; theme=2.51.2; module:VirtoCommerce.Xapi=3.1012.3; module:VirtoCommerce.XCart=3.1021.2; module:VirtoCommerce.Marketing=3.1005.0; module:VirtoCommerce.Customer=3.1011.1; module:VirtoCommerce.ProfileExperienceApiModule=3.1009.1; module:VirtoCommerce.Tax=3.1003.0; store=B2B-store; setting:Stores.TaxCalculationEnabled=true
    at: 2026-10-08T11:37:57.320Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: same rules; maintainer set has 8 permissions here."
---
Passing organization_id (or organizationId - either casing works) together with storeId on the storefront password grant scopes the token to that organization, and me.permissions returns that membership's role: one user who was maintainer of A and employee of B got 11 permissions for A and 2 (storefront:organization:view, storefront:user:view) for B on vcst_qa, 8 and 2 on vcptcore_stable.

Omitting organization_id does NOT reduce a token to the two view permissions. The token takes the role of the organization that comes first in the contact's organizations array AS STORED (which is not always the order it was written in), and me.contact.organizationId names that organization: a single-organization maintainer gets the full maintainer set, a single-organization employee gets the two view permissions, and a two-organization user gets whichever role the first stored organization gives. Whether the role is held per membership or globally on the account made no difference. Observed live 2026-10-08 on vcst_qa (11 permissions for a maintainer) and vcptcore_stable (8). So a token that "lacks xapi:my_organization:edit" points at the contact's first organization, not at a missing parameter by itself.
