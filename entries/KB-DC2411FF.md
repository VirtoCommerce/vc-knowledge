---
id: KB-DC2411FF
subject: "The impersonate grant on /connect/token gets no organization fallback: an organization_id the target is locked, Rejected or not a member in is refused with the org-specific code"
plane: experiential
question: What does POST /connect/token grant_type=impersonate return when organization_id names an organization the target user is locked or blocked in?
questions:
  - text: Does login on behalf fall back to another organization when the requested one is blocked for the customer?
  - text: Is the VCST-6132 credential sign-in organization fallback applied to impersonation?
status: active
appliesTo:
  - axis: feature
    value: organization-fallback
  - axis: grant
    value: impersonate
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 44afaf683e598288
    conditions: module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; platform=3.1084.0-pr-3140; store=B2B-store
    at: 2026-10-19T22:11:46.268Z
    by: session:1b3fc42e
    who: kutasinaelena
    note: "distinct: KB-3CBA0D5F covers the otp_email, password and refresh_token grants; this is the impersonate (login on behalf) grant, which it does not mention."
---
Admin bearer token, POST /connect/token grant_type=impersonate, user_id = a disposable buyer who belongs to org A and org B, organization_id = A. Both active: 200, organization_id claim A. A membership locked: 400 invalid_grant, errors[] one entry user_is_locked_in_organization, no token. A membership Rejected: 400 user_is_rejected_in_organization. A removed from the contact: 400 invalid_organization_id. The accessible org B is never substituted, unlike the password and otp_email grants on the same build. Each refusal writes a sign-in log row Impersonation / succeeded false / NotAllowed. Matches source: the impersonate block runs the token request validators with no SignInResult, and PR #321 allows the fallback only when SignInResult is set.
