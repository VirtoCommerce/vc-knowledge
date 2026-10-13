---
id: KB-ACC12F26
subject: On Punchout 3.1000.0-pr-1 up to build fbb9 a punchout user mapping accepted an unvalidated userId (privilege-escalation sign-in chain); from the fix commit 12dd178 (build ca33 onward) the mapping endpoint and the punchout grant reject it
plane: experiential
question: Does POST /api/punchout-user-mappings validate userId against the contact, and can a punchout mapping bind an administrator account to escalate privilege?
questions:
  - text: Can someone who is only allowed to create punchout mappings end up signed in as an administrator?
  - text: POST /api/punchout-user-mappings userId validation administrator account
  - text: Does the punchout grant on /connect/token issue a token with role __administrator when the mapping points at an admin user?
  - text: "Which identity does a punchout sign-in use: the mapping's userId or its memberId contact?"
  - text: Does PunchoutGrantTypeHandler reject administrator or mismatched-contact users before issuing a token?
concepts:
  - id: access-token
  - id: permission
  - id: sign-in
status: active
appliesTo:
  - axis: module
    value: virtocommerce.punchout
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/punchout-user-mappings
  - coordinate: POST /connect/token
  - coordinate: POST /api/punchout/cxml
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T21:01:00.412Z
    by: session:527ae7ed
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-08T20:59:48.518Z
    by: session:42875f89
    who: kutasinaelena
    contradicts: true
    note: On Punchout module 3.1000.0-pr-1-ca33 (2026-10-08) POST /api/punchout-user-mappings with an administrator userId returns 400 ("The user is not a security account of the specified contact" when memberId is another contact; "UserId and MemberId are required" without memberId); PUT switching userId to an admin also 400. A userId of a different contact's account is rejected the same way. The entry was true on build pr-1-fbb9 (before the fix commit 12dd178 "add grant validation, user mapping validation"); the grant handler now also refuses IsAdministrator / non-Customer users.
    resolved: version-scoped
    resolvedAt: 2026-10-13T08:01:53.220Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "Both sides are right on their own build: commit 12dd178 (2026-10-08) added the administrator/non-Customer grant check and the mapping validation; it is absent from fbb9 (entry) and present in ca33 (dispute), per the public source and the compare."
---
On vcptcore_qa1 (Punchout 3.1000.0-pr-1-fbb9), POST /api/punchout-user-mappings accepts a caller-supplied userId that is NOT validated against the mapping's memberId/contact and is NOT rejected for administrator accounts. After mapping an administrator account's userId to an external identity (punchout:create is sufficient), a normal anonymous cXML PunchOutSetupRequest (POST /api/punchout/cxml) + grant_type=punchout redeem on /connect/token returns HTTP 200 with an access_token carrying role=__administrator and channelId=punchout; /api/platform/security/currentuser then reports admin / isAdministrator:true. The signed-in principal is the mapping's userId (memberId does not constrain identity): mapping USER2's account onto USER's contact memberId signed in as USER2. PunchoutGrantTypeHandler.ValidateGrantAsync only checks user-exists / not-locked-out / CanSignInAsync, so an administrator passes. Net: a back-office user holding only punchout:create can obtain a full administrator token via an anonymous-reachable sign-in chain (privilege escalation). Candidate Critical bug; relates to VC-B2B-003 and PROPOSED-BL-PUNCHOUT-002. This was fixed within the same PR before release: vc-module-punchout commit 12dd178 (2026-10-08, 'add grant validation, user mapping validation') adds to PunchoutGrantTypeHandler.ValidateGrantAsync a refusal of user.IsAdministrator and user.UserType != Customer, and adds validation in PunchoutUserMappingController. The fix is absent from build fbb9 (2026-10-01, where the entry was observed) and present in build ca33 (2026-10-08, where the dispute observed POST /api/punchout-user-mappings with an administrator or cross-contact userId returning 400). So the escalation holds only on pre-fix pr-1 builds; no released Punchout build carries it. PR #1 (feat/VCST-5886) is closed.
