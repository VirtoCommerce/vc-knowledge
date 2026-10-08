---
id: KB-D8FDFA6F
subject: On /company/members a contact whose contact-level status is null has its NAME cell replaced by the literal text "Invite sent"; its status badge reads Active from theme 2.55 and Inactive before
plane: experiential
question: Why does the storefront company members list show "Invite sent" where a member's name should be, and why is that row still Active?
questions:
  - text: Why does my company members page say Invite sent instead of a colleague's name?
  - text: As a company maintainer, why does a row with no contact status still show a green Active tick?
  - text: Which field does the storefront members roster use for the Active badge, status or statusInOrganization?
  - text: What does the members roster render in the Name column when the contact's own status is null though the API returns the full name?
  - text: How is a null contact status resolved to an effective organization status before it reaches the roster?
concepts:
  - id: organization-member
  - id: membership-status
  - id: organization-invitation
status: active
appliesTo:
  - axis: principal
    value: org-maintainer
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /company/members
  - coordinate: Query.organization.contacts
  - coordinate: ContactType.status
  - coordinate: ContactType.statusInOrganization
  - coordinate: ContactType.fullName
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-22T08:33:33.895Z
    by: session:0d74f522
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T10:07:31.572Z
    by: session:8dcbfefd
    who: Dan-BV
    contradicts: true
    note: "2026-10-05 Customer 3.1026.0: pending invitees read statusInOrganization Invited, not Approved, and contact status Invited, not null. The UI badge was not checked (API only)."
    resolved: dispute-wrong
    resolvedAt: 2026-10-08T10:18:31.955Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: It checked pending invitees (status Invited); this entry is about contacts whose status is null, which still render Invite sent (live G2, G3 2026-10-08); the badge is now scoped by theme.
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.Customer=3.1011.1; module:VirtoCommerce.ProfileExperienceApiModule=3.1009.1; module:VirtoCommerce.Xapi=3.1012.3; theme=2.51.2; store=B2B-store; role=Organization maintainer
    at: 2026-10-08T10:18:02.466Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Contact created over REST with a name and status null, 2026-10-08: Name cell \"Invite sent\", badge Inactive (theme 2.51.2 badge reads contact.status). A named contact with status Invited also shows \"Invite sent\"."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1020.0; module:VirtoCommerce.Xapi=3.1026.0; theme=2.59.0; store=B2B-store; role=Organization maintainer
    at: 2026-10-08T10:18:03.256Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Contact created over REST with a name and status null, 2026-10-08: Name cell \"Invite sent\", badge Active; xAPI statusInOrganization Approved with membership status null. A named contact with status Invited also shows \"Invite sent\"."
---
The storefront roster replaces a contact's name with the literal "Invite sent" whenever the contact's status is empty OR Invited - the rule is convertToExtendedContact in vc-frontend (unchanged since #929, 2024). So a contact whose status is null renders "Invite sent" even though the API returns its name in name, firstName, lastName and fullName; a maintainer auditing the roster sees only the email for that row, and a test asserting the rendered name against the API name fails for this class of contact. Such a contact is usually one created over REST or by a seeder without a status, not a pending invitation (a real invitee's status is Invited, see KB-BBBC4225).

The status badge of that row differs by theme. From theme 2.55 the badge is isLockedInOrganization ? Blocked : (statusInOrganization ?? status), and xAPI resolves statusInOrganization to Approved when neither the membership nor the contact carries a status, so the row shows the green Active tick. Up to theme 2.54 the badge reads contact.status only, so the same contact shows Inactive. A model predicting the badge from the raw contact.status is right on theme <= 2.54 and wrong from 2.55. Observed live 2026-10-08 on both (theme 2.59.0 and 2.51.2).
