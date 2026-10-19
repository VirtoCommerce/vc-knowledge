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
---
On vcptcore_qa1 a disposable contact in orgs A, B, C (read back in order B, A, C) with DefaultOrganizationId=C: with A locked or A Rejected, POST /connect/token (otp_email and password) with organization_id=A returned 200 with organization_id claim B, not the default C. Setting CurrentOrganizationId=C beforehand did not change it: claim B, and CurrentOrganizationId was rewritten to B. Also: A removed from the contact + B Rejected gave 200 without an organization_id claim on both grants; A locked + B active, password with organization_id=A gave 200 claim B (not 400).
