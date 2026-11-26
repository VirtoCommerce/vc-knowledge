---
id: KB-02ACDBF7
subject: Profile xAPI inviteUser and changeOrganizationContactRole refuse any role outside the store's MembershipRolesWhitelist with RoleNotAllowed; a whitelisted role succeeds
plane: experiential
question: Can an organization maintainer assign a back-office platform role (Store manager, Order manager) through inviteUser or changeOrganizationContactRole?
questions:
  - text: Does the storefront role allow-list block privilege escalation through Profile xAPI?
  - text: What does inviteUser return for a role not in Customer.MembershipRolesWhitelist?
  - text: Is VCST-7006 (any platform role via inviteUser) blocked on mainline Profile xAPI?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.inviteUser
  - coordinate: Mutation.changeOrganizationContactRole
  - coordinate: /company/members
evidence:
  - method: observation
    deployment: vcst_qa
    build: 6feb83b80e6f1f12
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; module:VirtoCommerce.ProfileExperienceApiModule=3.1028.0; store=B2B-store; role=Organization maintainer
    at: 2026-11-26T09:32:42.055Z
    by: session:779b94fc
---
2026-11-26, B2B-store, storefront password-grant token of an Organization maintainer, store whitelist holding 7 roles. Mutation.inviteUser with roleIds ["Store manager"] or ["Order manager"] (by name) returned HTTP 200 {succeeded:false, errors:[{code:"RoleNotAllowed", description:"Role 'Store manager' is not allowed for company members of this store"}]} and created no contact or account. Mutation.changeOrganizationContactRole(command:{memberId:<contact id of a pending invitee in the same org>, roleIds:["Store manager"|"Order manager"]}) returned the same RoleNotAllowed; with roleIds ["purchasing-agent"] (whitelisted) it returned succeeded:true. The storefront Invite member dialog lists exactly the GetAssignableCompanyRoles result (7 whitelisted roles) and the UI invite with org-employee / org-maintainer succeeds. Note: the command takes memberId (contact id); passing userId fails GraphQL validation.
