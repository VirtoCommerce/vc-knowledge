---
id: KB-DA14E8B7
subject: "Block then Unblock on a pending invitee: on builds up to PEA 3.1007 it irreversibly ends the invitation (Invited -> Locked -> Approved); from PEA 3.1008 it only locks the organization membership"
plane: experiential
question: What happens to a pending organization invitation if the maintainer blocks and then unblocks that row?
questions:
  - text: If I block and then unblock someone I invited but who never joined, can they still accept the invitation?
  - text: As a company maintainer, can I undo blocking a pending invitee, or re-invite the same email afterwards?
  - text: Why does an unblocked invitee show as Active on the roster though they never registered?
  - text: What do lockOrganizationContact and unlockOrganizationContact write to contact status and account lockoutEnd for an invited contact?
  - text: Does deleting a blocked invitee from the organization free the email address for a new invitation?
  - text: Our test site can't send emails - can an invited colleague still be activated?
  - text: Does blocking and then unblocking an invited member turn them into an active member?
  - text: What happens to contact status and lockoutEnd when a pending invitee is blocked and unblocked?
  - text: Why does passwordHash look null on one user endpoint but populated on the members endpoint?
concepts:
  - id: member-block
  - id: organization-invitation
  - id: membership-status
  - id: account-lockout
status: active
appliesTo:
  - axis: principal
    value: org-maintainer
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: GET /company/members
  - coordinate: Mutations.lockOrganizationContact
  - coordinate: Mutations.unlockOrganizationContact
  - coordinate: ContactType.status
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:24:16.232Z
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-11T18:01:08.559Z
    by: session:8df1fb2c
    splitFrom: KB-4D082C89
    mergedFrom: KB-51459ABB
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:13:03.274Z
    by: session:f00f5968
    splitFrom: KB-4D082C89
    mergedFrom: KB-51459ABB
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:16:29.143Z
    contradicts: true
    note: "The state mapping in this entry held exactly (I confirmed it minutes earlier). The final clause does not: 'a deployment with no reachable mailbox cannot move a member out of this state from the storefront side'. It can, and easily. Block user followed by Unblock user on the roster row moves the pending invitee out of the invited state with no mailbox involved: contact.status goes Invited -> Locked -> Approved, and Unblock resets the account lockoutEnd from 9999-12-31 to 0001-01-01. The account still has passwordHash null and emailConfirmed false, so completing the registration is NOT the only thing that clears the lockout, and the roster afterwards reports the person as Active."
    splitFrom: KB-4D082C89
    mergedFrom: KB-51459ABB
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T16:21:18.689Z
    contradicts: true
    note: "Second contradicting observation, 2026-09-15 on platform 3.1007.27. The member this entry was written from now presents in the shape the FIRST dispute predicted, not the shape the body describes: roster name 'Invited User' (a real name, not 'Invite sent'), roster status Active (not Invited), Admin Locked state Unlocked (not Locked), Admin Status PendingApproval (not empty). That is consistent with the Block-then-Unblock cycle the first dispute recorded, so it does NOT show the body wrong about a FRESH invitation - I have not observed one on this version and am not claiming to. What it does contradict outright is 'the account already carries the role chosen in the invite dialog': this account holds ZERO roles and the roster Role cell is blank. Either the Block/Unblock cycle strips the role, or the claim was never right; not established, and worth one experiment on a fresh invite. Practical warning of the entry is untouched and still right: do not read the roster as sign-in state."
    splitFrom: KB-4D082C89
    mergedFrom: KB-51459ABB
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T17:53:02.256Z
    contradicts: true
    note: "Third contradicting observation, 2026-09-15, platform 3.1007.27. The first dispute note on this entry states the account 'still has passwordHash null'. It is NOT null: GET /api/members/{id} returns securityAccounts with passwordHash populated, an 84-character ASP.NET Identity v3 value, on this same invited account. The earlier note was almost certainly written from GET /api/platform/security/users/{userName}, which strips the field via ReduceUserDetails - so the two observations are of the same account through two endpoints that redact differently, and only one of them tells the truth. Recorded separately as its own fact. This does not touch the rest of the note, whose account of the Block-then-Unblock path still stands."
    splitFrom: KB-4D082C89
    mergedFrom: KB-51459ABB
    resolved: claim-amended
    resolvedAt: 2026-10-08T10:18:27.061Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: "Body states it: the users endpoint always returns passwordHash null, /api/members/{id} shows the real value; re-observed live on G2 2026-10-08."
  - method: observation
    deployment: vcst_qa
    at: 2026-09-22T08:34:09.138Z
    by: session:0d74f522
    contradicts: true
    note: "The storefront-roster half of the body does not hold on vcst_qa 2026-09-22, storefront build 2.58.0-pr-2467-89a3-89a38239, org AGENT-TEST-Org-AcmeCorp-20260310 (105c2c4e-23be-4258-8691-568a0ff190be), read-only, signed in as org maintainer acme_store_maintainer_1@acme.com. The body says a row in this state \"shows the literal name 'Invite sent' with an empty real name ... and status 'Invited'\". Two of those three clauses failed together on the same row.\n\n(1) The real name is NOT empty. The row rendering \"Invite sent\" is contact 584bf5d5-f0f1-45ab-9630-d0faa6eca0d5, and the GetOrganizationContacts response for it carries name \"Sam Store\", firstName \"Sam\", lastName \"Store\" AND fullName \"Sam Store\" - four populated fields. So the page is not falling back to a placeholder because the name is missing; it is actively substituting \"Invite sent\" over a name the API supplied. That matters because the entry's phrasing invites a reader to treat \"Invite sent\" as evidence of an empty name, and it is not.\n\n(2) The status badge is NOT \"Invited\". It is the green tick, img alt \"Active\", the same badge the 14 ordinary members carry. The row's payload is status=null, statusInOrganization=\"Approved\", isLockedInOrganization=false, and the Active column consumes statusInOrganization, so the three-tier effective-status fallback (membership.Status ?? contact.Status ?? Approved) materialises the null as \"Approved\" server-side before it reaches the page. On this stand the roster therefore does NOT have a third value distinguishing a pending invitee - \"Invited\" is reachable in the badge map, but not by this row.\n\n(3) The role clause survives here, against the entry's own second dispute: this row shows a populated role, \"Sales Representative\", and the payload carries it in both rolesInOrganization and securityAccounts[0].roles.\n\nCaveat on what this does and does not settle: I did not create a fresh invitation (read-only pass), so I cannot say this contact is in the same lifecycle state the entry was written from - status=null is consistent with a pending invite but I did not establish it. All of the entry's own observations are vcptcore_stable, so a build difference is a live possibility rather than the entry being wrong where it was taken. What is certain is that on this build, a \"Invite sent\" row reads Active and has a full name behind it. Captured the positive form separately as KB-D8FDFA6F, which overlaps this entry's storefront half - if a human prefers, that capture should be folded into this entry rather than standing alone."
    splitFrom: KB-4D082C89
    mergedFrom: KB-51459ABB
    resolved: split
    resolvedAt: 2026-10-08T10:18:27.864Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/invitations-20261008
    resolution: About the roster row of a seeded contact (KB-BBBC4225, KB-D8FDFA6F), not about block/unblock.
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.Customer=3.1011.1; module:VirtoCommerce.ProfileExperienceApiModule=3.1009.1; module:VirtoCommerce.Xapi=3.1012.3; theme=2.51.2; store=B2B-store; role=Organization maintainer
    at: 2026-10-08T10:17:59.121Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Fresh invitee, Block then Unblock from the roster, 2026-10-08: contact status stayed Invited throughout; only isLockedInOrganization toggled (true then false); account lockoutEnd 9999-12-31 unchanged; membership role kept. GET /api/platform/security/users/{name} returned passwordHash null for every account, /api/members/{id} reflected the real value."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ProfileExperienceApiModule=3.1020.0; module:VirtoCommerce.Xapi=3.1026.0; theme=2.59.0; store=B2B-store; role=Organization maintainer
    at: 2026-10-08T10:18:00.011Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Fresh invitee, 2026-10-08: the roster menu of an Invited row offers Resend invite / Revoke invite and no Block user; locked via the same LockOrganizationContact the roster sends, then unblocked from the roster: contact status stayed Invited, membership isLocked toggled, account lockoutEnd unchanged, membership role kept; the row returned to Invited."
---
What Block user / Unblock user do to a pending invitee depends on the build. Which build a stand runs decides this. G1 = ProfileExperienceApi (PEA) <= 3.1007, Customer 3.1000.x, theme <= 2.51.0 (vcptcore_stable until 2026-10-07). G2 = PEA 3.1008-3.1014, Customer 3.1010-3.1020, theme 2.51.2-2.54. G3 = PEA >= 3.1015, Customer >= 3.1021, theme >= 2.55.

On G1 (lockOrganizationContact / unlockOrganizationContact up to PEA 3.1007) the pair writes the CONTACT's status field, where an outstanding invitation also lives, so it is a one-way door that silently ends the invitation. Block overwrites contact.status Invited -> Locked and locks the security account; the row stops showing the 'Invite sent' placeholder. Unblock writes contact.status = Approved and unlocks the account, resetting lockoutEnd from the invitation's 9999-12-31 sentinel to 0001-01-01. emailConfirmed stays false - nobody registered - yet the roster then reports the person as Active, and Admin shows Locked state Unlocked. Nothing on either surface puts the row back to Invited: Delete only detaches the contact, the account survives holding the address, and re-inviting that address is refused as a duplicate. This G1 behaviour is consistent with the G1 source and the September observations; no stand runs G1 any more, so it was not re-observed live.

From PEA 3.1008 (#135, VCST-5028) both mutations lock and unlock only the ORGANIZATION MEMBERSHIP. Observed live 2026-10-08 on G2 and G3: after Block and after Unblock the contact status stays Invited, the account and its lockoutEnd are untouched, the membership keeps its role, and the invitation survives. On G3 the roster's menu for an Invited row offers Resend invite / Revoke invite and no Block user at all.

Separately, on every build: GET /api/platform/security/users/{userName} always returns passwordHash null (the field is stripped), while GET /api/members/{id} securityAccounts[].passwordHash shows whether a password exists - a null read from the former is not proof that no password exists.
