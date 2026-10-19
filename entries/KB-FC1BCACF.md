---
id: KB-FC1BCACF
subject: The credential sign-in fallback away from a blocked organization picks the first accessible organization in the contact's list, ignoring DefaultOrganizationId and CurrentOrganizationId, and then overwrites CurrentOrganizationId with it
plane: experiential
question: When a sign-in falls back from a blocked organization, does it prefer the contact's default or current organization?
questions:
  - text: Which organization does otp_email or password fallback pick when the buyer has three organizations and a default?
concepts:
  - id: organization
  - id: organization-membership
status: active
appliesTo:
  - axis: feature
    value: organization-fallback
  - axis: persona
    value: multi-org
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 5b60cc99a102c4af
    conditions: module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; platform=3.1084.0-pr-3140
    at: 2026-10-19T21:55:45.993Z
    by: session:1b3fc42e
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 5b60cc99a102c4af
    conditions: module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; platform=3.1084.0-pr-3140
    at: 2026-10-19T22:11:27.899Z
    by: session:1b3fc42e
    bodyHash: 8678ef894516
    who: kutasinaelena
    note: "Re-observed the parts this fixture can separate: CurrentOrganizationId was A before and B after an otp_email fallback from locked A; A removed + B Rejected -> 200 without organization_id claim on otp_email and password; A locked + B active, password org=A -> 200 claim B. A sign-in that resolves no organization (both locked) left CurrentOrganizationId unchanged (A). Fixture had no default org, so the default-vs-list-order part was not re-tested."
  - method: observation
    deployment: vcptcore_qa1
    build: 44afaf683e598288
    conditions: module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; store=B2B-store
    at: 2026-10-19T22:34:54.209Z
    by: session:1b3fc42e
    bodyHash: 8678ef894516
    who: kutasinaelena
    note: "Seeded two-org persona, password grant org=A with A Invited (and A Invited + permanently locked): 200 claim B, and GET /api/members showed CurrentOrganizationId rewritten A -> B. Single-org buyer with its only org temp-locked: 200 with no claim and CurrentOrganizationId stayed null."
---
On vcptcore_qa1 a disposable contact in orgs A, B, C (read back in order B, A, C) with DefaultOrganizationId=C: with A locked or A Rejected, POST /connect/token (otp_email and password) with organization_id=A returned 200 with organization_id claim B, not the default C. Setting CurrentOrganizationId=C beforehand did not change it: claim B, and CurrentOrganizationId was rewritten to B. Also: A removed from the contact + B Rejected gave 200 without an organization_id claim on both grants; A locked + B active, password with organization_id=A gave 200 claim B (not 400).
