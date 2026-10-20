---
id: KB-2EE43E7A
subject: Storefront OTP sign-in records in the Security Sign-in log carry organizationId and organizationName null, on success and on an organization-refused failure (reason Unknown)
plane: experiential
question: Does the Admin Sign-in log record the organization a storefront OTP sign-in landed in or was refused for?
questions:
  - text: Why is the Organization Id column empty in the Admin Sign-in log for buyer sign-ins?
  - text: What failure reason does the sign-in log show when an OTP sign-in is refused with user_is_rejected_in_organization?
  - text: Does the Sign-in record blade show an organization field?
status: active
appliesTo:
  - axis: grant
    value: otp_email
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/sign-in-log/search
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: e7a47c5e8f6de4e7
    conditions: platform=3.1084.0-pr-3140-9092; theme=2.61.0-pr-2564-69d4; module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f
    at: 2026-10-20T06:30:42.169Z
    by: session:1b3fc42e
    who: kutasinaelena
---
vcptcore_qa1, 2026-10-20. Disposable buyer with memberships in three orgs; two successful storefront OTP sign-ins (one landing on a fallback organization, token claim organization_id set) and one refused by POST /connect/token grant_type=otp_email with 400 invalid_grant code user_is_rejected_in_organization. POST /api/platform/security/sign-in-log/search returned all three rows with signInType OTP, memberId set, organizationId null and organizationName null; the refused row had succeeded false, failureReason "Unknown". Admin Security > Sign-in log > View all records: grid has Organization Id / Organization Name columns hidden by default (enabled via grid menu) and they are empty for all three rows; the Sign-in record blade lists Date, User, Reason, Type, User ID, Member ID, IP address, Host, User agent, Store and no organization field. The two org-switch actions in between produced no rows.
