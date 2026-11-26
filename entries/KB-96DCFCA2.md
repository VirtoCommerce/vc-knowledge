---
id: KB-96DCFCA2
subject: changeOrganizationContactRole on a contact of another organization is refused with a GraphQL error whose extensions.code is Forbidden; the target's roles stay unchanged
plane: experiential
question: What does changeOrganizationContactRole return when an organization maintainer targets a contact of a different organization?
questions:
  - text: Can an org maintainer change the role of a member of another organization via xAPI?
  - text: Is cross-org changeOrganizationContactRole Unauthorized or Forbidden?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeOrganizationContactRole
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; store=B2B-store; role=Organization maintainer
    at: 2026-11-26T07:04:01.063Z
    by: session:779b94fc
    who: kutasinaelena
---
2026-11-26 regression: with a storefront token of an Organization maintainer scoped to organization A, Mutation.changeOrganizationContactRole(command:{memberId:<contact that belongs only to organization B>, roleIds:["org-maintainer"]}) returned HTTP 200, data null and one errors[] entry with extensions.code = Forbidden. Reading organization B's contacts before and after showed the target's rolesInOrganization unchanged (Purchasing agent). Earlier observations of the same refusal recorded the code as Unauthorized, so an assertion should check errors[] non-empty plus the unchanged target, not a specific code.
