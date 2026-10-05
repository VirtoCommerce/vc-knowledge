---
id: KB-ACC12F26
subject: A punchout user mapping accepts an unvalidated userId (incl. an administrator account), yielding a privilege-escalation sign-in via the anonymous cXML + punchout grant chain
plane: experiential
question: Does POST /api/punchout-user-mappings validate userId against the contact, and can a punchout mapping bind an administrator account to escalate privilege?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.punchout
  - axis: surface
    value: platform-api
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
---
On vcptcore_qa1 (Punchout 3.1000.0-pr-1-fbb9), POST /api/punchout-user-mappings accepts a caller-supplied userId that is NOT validated against the mapping's memberId/contact and is NOT rejected for administrator accounts. After mapping an administrator account's userId to an external identity (punchout:create is sufficient), a normal anonymous cXML PunchOutSetupRequest (POST /api/punchout/cxml) + grant_type=punchout redeem on /connect/token returns HTTP 200 with an access_token carrying role=__administrator and channelId=punchout; /api/platform/security/currentuser then reports admin / isAdministrator:true. The signed-in principal is the mapping's userId (memberId does not constrain identity): mapping USER2's account onto USER's contact memberId signed in as USER2. PunchoutGrantTypeHandler.ValidateGrantAsync only checks user-exists / not-locked-out / CanSignInAsync, so an administrator passes. Net: a back-office user holding only punchout:create can obtain a full administrator token via an anonymous-reachable sign-in chain (privilege escalation). Candidate Critical bug; relates to VC-B2B-003 and PROPOSED-BL-PUNCHOUT-002.
