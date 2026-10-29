---
id: KB-ED1C3F63
subject: "An operator whose loginOnBehalf comes only from an organization-scoped role can impersonate a user of another organization: the impersonate grant does not apply the role's organization scope"
plane: experiential
question: Can a storefront operator whose loginOnBehalf comes from an organization-scoped role (OrganizationMembership) impersonate a user from a different organization via POST /connect/token grant_type=impersonate?
questions:
  - text: Is login on behalf limited to the operator's own organization?
  - text: Does an org-scoped Organization maintainer role let you impersonate users in other companies?
  - text: Cross-organization impersonation from the storefront
concepts:
  - id: impersonation
  - id: organization-membership
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: /account/impersonate/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: role=Organization maintainer (org-scoped only); state:target=member of a different organization
    at: 2026-10-29T10:55:35.790Z
    by: session:423e9746
    who: Lenajava1
    note: "distinct: KB-28DB8660 is an operator with a GLOBAL role holding loginOnBehalf, which the platform docs say is unrestricted. This entry is an operator whose loginOnBehalf comes ONLY from an organization-scoped role (OrganizationMembership), which the storefront docs describe as limited to colleagues in the same organization."
---
On vcst_qa on 2026-10-29: the operator @td(IMPERSONATION_ADMIN) is a non-admin Customer. Its role Organization maintainer, which carries platform:security:loginOnBehalf, is assigned only org-scoped, through an OrganizationMembership on ORG-002; the seed keeps B2B roles off the account's global roles. It signed in on the storefront and opened /account/impersonate/{userId} for @td(OTHER_ORG_USER), a Purchasing agent of a different organization (ORG-003) with no ORG-002 membership. POST /connect/token grant_type=impersonate&user_id=<target> returned 200. The storefront reloaded with the header 'Impersonation Operator logged in as <target>' and showed the target's organization. So the organization scope of an org-scoped loginOnBehalf is not applied to the impersonate target. In source: the Customer OrganizationIdClaimProvider adds the current organization's org-scoped role permissions to the access token as plain permission claims, and the platform impersonate grant checks loginOnBehalf with AuthorizeAsync(User, null, ...) and the target only for existence. The VirtoOZ storefront user guide (Company Members, Log in on behalf of company member) describes login on behalf by an organization member as acting 'as a colleague in the same organization'. A GLOBAL loginOnBehalf holder reaching any user is documented as intended (KB-28DB8660, the platform docs warning). Not marked as a defect: no product decision yet.
