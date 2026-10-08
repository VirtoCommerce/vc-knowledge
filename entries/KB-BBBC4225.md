---
id: KB-BBBC4225
subject: The storefront roster shows a pending invitee as 'Invite sent' with status Invited
plane: experiential
question: How does a pending invitee appear on the storefront company members list?
questions:
  - text: Why does someone I invited show as 'Invite sent' on our company members page?
  - text: Does the members list distinguish a pending invitation from an active member?
  - text: Is 'Invite sent' a placeholder for an empty name or a substitution over a real one?
  - text: Which field does the roster status badge read, and can it ever render Invited?
concepts:
  - id: organization-invitation
  - id: membership-status
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /company/members
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-11T18:01:08.559Z
    by: session:8df1fb2c
    splitFrom: KB-4D082C89
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:13:03.274Z
    by: session:f00f5968
    splitFrom: KB-4D082C89
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:16:29.143Z
    contradicts: true
    note: "The state mapping in this entry held exactly (I confirmed it minutes earlier). The final clause does not: 'a deployment with no reachable mailbox cannot move a member out of this state from the storefront side'. It can, and easily. Block user followed by Unblock user on the roster row moves the pending invitee out of the invited state with no mailbox involved: contact.status goes Invited -> Locked -> Approved, and Unblock resets the account lockoutEnd from 9999-12-31 to 0001-01-01. The account still has passwordHash null and emailConfirmed false, so completing the registration is NOT the only thing that clears the lockout, and the roster afterwards reports the person as Active."
    splitFrom: KB-4D082C89
    resolved: split
    resolvedAt: 2026-10-08T10:18:28.673Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: Copied here by the split of KB-4D082C89; block/unblock is stated in KB-DA14E8B7, not in this entry.
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T16:21:18.689Z
    contradicts: true
    note: "Second contradicting observation, 2026-09-15 on platform 3.1007.27. The member this entry was written from now presents in the shape the FIRST dispute predicted, not the shape the body describes: roster name 'Invited User' (a real name, not 'Invite sent'), roster status Active (not Invited), Admin Locked state Unlocked (not Locked), Admin Status PendingApproval (not empty). That is consistent with the Block-then-Unblock cycle the first dispute recorded, so it does NOT show the body wrong about a FRESH invitation - I have not observed one on this version and am not claiming to. What it does contradict outright is 'the account already carries the role chosen in the invite dialog': this account holds ZERO roles and the roster Role cell is blank. Either the Block/Unblock cycle strips the role, or the claim was never right; not established, and worth one experiment on a fresh invite. Practical warning of the entry is untouched and still right: do not read the roster as sign-in state."
    splitFrom: KB-4D082C89
    resolved: split
    resolvedAt: 2026-10-08T10:18:29.492Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: Copied here by the split of KB-4D082C89; it concerns account roles (KB-972CF223), not the roster row.
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T17:53:02.256Z
    contradicts: true
    note: "Third contradicting observation, 2026-09-15, platform 3.1007.27. The first dispute note on this entry states the account 'still has passwordHash null'. It is NOT null: GET /api/members/{id} returns securityAccounts with passwordHash populated, an 84-character ASP.NET Identity v3 value, on this same invited account. The earlier note was almost certainly written from GET /api/platform/security/users/{userName}, which strips the field via ReduceUserDetails - so the two observations are of the same account through two endpoints that redact differently, and only one of them tells the truth. Recorded separately as its own fact. This does not touch the rest of the note, whose account of the Block-then-Unblock path still stands."
    splitFrom: KB-4D082C89
    resolved: split
    resolvedAt: 2026-10-08T10:18:30.294Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: Copied here by the split of KB-4D082C89; passwordHash is stated in KB-DA14E8B7, not in this entry.
  - method: observation
    deployment: vcst_qa
    at: 2026-09-22T08:34:09.138Z
    by: session:0d74f522
    contradicts: true
    note: "The storefront-roster half of the body does not hold on vcst_qa 2026-09-22, storefront build 2.58.0-pr-2467-89a3-89a38239, org AGENT-TEST-Org-AcmeCorp-20260310 (105c2c4e-23be-4258-8691-568a0ff190be), read-only, signed in as org maintainer acme_store_maintainer_1@acme.com. The body says a row in this state \"shows the literal name 'Invite sent' with an empty real name ... and status 'Invited'\". Two of those three clauses failed together on the same row.\n\n(1) The real name is NOT empty. The row rendering \"Invite sent\" is contact 584bf5d5-f0f1-45ab-9630-d0faa6eca0d5, and the GetOrganizationContacts response for it carries name \"Sam Store\", firstName \"Sam\", lastName \"Store\" AND fullName \"Sam Store\" - four populated fields. So the page is not falling back to a placeholder because the name is missing; it is actively substituting \"Invite sent\" over a name the API supplied. That matters because the entry's phrasing invites a reader to treat \"Invite sent\" as evidence of an empty name, and it is not.\n\n(2) The status badge is NOT \"Invited\". It is the green tick, img alt \"Active\", the same badge the 14 ordinary members carry. The row's payload is status=null, statusInOrganization=\"Approved\", isLockedInOrganization=false, and the Active column consumes statusInOrganization, so the three-tier effective-status fallback (membership.Status ?? contact.Status ?? Approved) materialises the null as \"Approved\" server-side before it reaches the page. On this stand the roster therefore does NOT have a third value distinguishing a pending invitee - \"Invited\" is reachable in the badge map, but not by this row.\n\n(3) The role clause survives here, against the entry's own second dispute: this row shows a populated role, \"Sales Representative\", and the payload carries it in both rolesInOrganization and securityAccounts[0].roles.\n\nCaveat on what this does and does not settle: I did not create a fresh invitation (read-only pass), so I cannot say this contact is in the same lifecycle state the entry was written from - status=null is consistent with a pending invite but I did not establish it. All of the entry's own observations are vcptcore_stable, so a build difference is a live possibility rather than the entry being wrong where it was taken. What is certain is that on this build, a \"Invite sent\" row reads Active and has a full name behind it. Captured the positive form separately as KB-D8FDFA6F, which overlaps this entry's storefront half - if a human prefers, that capture should be folded into this entry rather than standing alone."
    splitFrom: KB-4D082C89
    resolved: dispute-wrong
    resolvedAt: 2026-10-08T10:18:31.114Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: The "Invite sent" row it read (Sam Store) is a seeded contact with status null, not a pending invitee; a fresh invitee shows Invite sent and the Invited badge on G2 and G3 (live 2026-10-08).
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T10:07:31.572Z
    by: session:8dcbfefd
    who: Dan-BV
    note: "2026-10-05 Customer 3.1026.0, ProfileExperienceApiModule 3.1019.0: two pending invitees (account emailConfirmed false, never logged in) read contact.status Invited (REST) and status Invited, statusInOrganization Invited, isLockedInOrganization false (GraphQL organization.contacts)."
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.Customer=3.1011.1; module:VirtoCommerce.ProfileExperienceApiModule=3.1009.1; module:VirtoCommerce.Xapi=3.1012.3; theme=2.51.2; store=B2B-store; role=Organization maintainer
    at: 2026-10-08T10:18:00.867Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Fresh invitee row 2026-10-08: Name \"Invite sent\", Role Purchasing agent, badge Invited; xAPI status Invited, name empty."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1020.0; module:VirtoCommerce.Xapi=3.1026.0; theme=2.59.0; store=B2B-store; role=Organization maintainer
    at: 2026-10-08T10:18:01.678Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Fresh invitee row 2026-10-08: Name \"Invite sent\", Role Organization employee, badge Invited; xAPI status Invited, statusInOrganization Invited. The previously disputed \"Sam Store\" row is a seeded contact (status null, email confirmed, password set, created by admin), not an invitee."
---
On the storefront company members roster (/company/members) the row of a pending invitee shows the literal name 'Invite sent' - the invitee's contact has an empty name - with the chosen role, the email, and the status badge 'Invited', so the status column has a value beyond Active and Blocked. Observed live 2026-10-08 on two builds.

How the row is derived differs by build, the result does not. Which build a stand runs decides this. G1 = ProfileExperienceApi (PEA) <= 3.1007, Customer 3.1000.x, theme <= 2.51.0 (vcptcore_stable until 2026-10-07). G2 = PEA 3.1008-3.1014, Customer 3.1010-3.1020, theme 2.51.2-2.54. G3 = PEA >= 3.1015, Customer >= 3.1021, theme >= 2.55. The 'Invite sent' text comes from the storefront rule "contact.status empty OR Invited -> show the invite_sent placeholder" (unchanged since vc-frontend #929, 2024), so it also replaces the name of a contact whose status is null even when a real name exists (see KB-D8FDFA6F). The badge reads contact.status up to theme 2.54 and isLockedInOrganization ? Blocked : (statusInOrganization ?? status) from theme 2.55; for an invitee both resolve to Invited. The role shown comes from the security account on G1 and from the organization membership from Customer 3.1010.
