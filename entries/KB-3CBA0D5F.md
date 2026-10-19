---
id: KB-3CBA0D5F
subject: With the VCST-6132 fix, POST /connect/token grant_type=otp_email falls back to the buyer's remaining organization exactly like the password grant when the sent organization_id is removed, locked, Invited, Rejected or Deleted; refresh_token to a blocked org still fails
plane: experiential
question: After the VCST-6132 fix, which organization does an otp_email sign-in land in when the last-used organization_id is no longer usable?
questions:
  - text: Does code sign-in fall back to another organization when the buyer is locked in the last-used organization?
  - text: connect/token otp_email organization_id blocked membership fallback after vc-module-customer PR 321
  - text: Can a refresh_token organization switch fall back to another organization?
concepts:
  - id: otp
  - id: organization
  - id: sign-in
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: surface
    value: rest
  - axis: ticket
    value: vcst-6132
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/otp/request
  - coordinate: POST /api/customer/organization-memberships/{id}/lock
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: b18a795959ce0db8
    conditions: module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; store=B2B-store
    at: 2026-10-16T11:57:23.952Z
    by: session:93dd4302
    who: kutasinaelena
    note: "distinct: KB-ED213173 is the pre-fix behaviour on Customer 3.1029.0 (otp_email fails); this is the post-fix behaviour on the PR #321 build (otp_email falls back), across all five blocked states plus refresh_token."
  - method: observation
    deployment: vcptcore_qa1
    build: 5b60cc99a102c4af
    conditions: module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f; platform=3.1084.0-pr-3140; store=B2B-store
    at: 2026-10-19T22:11:25.273Z
    by: session:1b3fc42e
    bodyHash: 9e8fee29dd1b
    who: kutasinaelena
    note: "Re-observed 2026-10-19 with a fresh disposable buyer (A,B). otp_email org=A: removed/Invited/Rejected/Deleted/locked in A -> 200 claim B; both locked -> 200 no organization_id claim; both Rejected/Invited/Deleted -> 400 invalid_grant with exactly one errors[] entry (user_is_rejected_in_organization / user_invitation_pending_in_organization / user_is_removed_from_organization), identical on password. A removed + B Rejected and A locked + B Rejected -> 200 no claim on both grants. refresh_token to A gives user_is_locked_in_organization / invalid_organization_id only when the refresh token was issued AFTER the membership change: a membership write terminates the user's sessions, so a refresh token from before it fails with code-less invalid_grant 'no longer valid' (see KB-D1D30720)."
---
On vcptcore_qa1 running VirtoCommerce.Customer 3.1030.0-pr-321-c54f (PR build of vc-module-customer#321), a disposable buyer with memberships in org A and org B signed in with organization_id=A on every call. For the otp_email grant (fresh code per scenario from POST /api/otp/request) and the password grant, results were identical. Both active: 200 with claim A. Removed from A: 200 with claim B. Locked in A: 200 with claim B. Membership Invited, Rejected or Deleted in A: 200 with claim B. Locked in both: 200 with no organization_id claim. Rejected in both: 400 invalid_grant, code user_is_rejected_in_organization. grant_type=refresh_token with organization_id=A while locked in A: 400 invalid_grant, code user_is_locked_in_organization, so an explicit switch still gets no fallback. Before the fix (3.1029.0) the removed-from-A otp_email case returned 400 invalid_grant; see KB-ED213173.
