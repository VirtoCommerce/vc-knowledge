---
id: KB-262ADDFB
subject: A password-grant token request for a temporarily locked-out account returns 400 invalid_grant with code user_is_temporary_locked_out, and the response body carries lockoutSecondsRemaining (null) and userId (null)
plane: experiential
question: What does POST /connect/token grant_type=password return for an account that is temporarily locked out?
questions:
  - text: Does the password grant response include lockoutSecondsRemaining like the OTP grant does?
  - text: What error code does the token endpoint give when an account is temporarily locked out after failed attempts?
  - text: Why does the admin token request return 400 user_is_temporary_locked_out?
status: active
appliesTo:
  - axis: feature
    value: account-lockout
  - axis: grant
    value: password
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1088.0-pr-3133-23a5-vcst-5904; module:VirtoCommerce.MFA=3.1000.0-pr-1-41b3; module:VirtoCommerce.OTP=3.1001.0-pr-3-d978; account=platform administrator
    at: 2026-10-21T16:14:12.727Z
    by: session:5fa7c31a
    who: Lenajava1
    note: "distinct: KB-444EF3F4 is the PERMANENT platform lock (code user_is_locked_out). This is the TEMPORARY failed-attempt lockout, which uses a different code (user_is_temporary_locked_out) and adds the response-shape detail that lockoutSecondsRemaining is present but null on the password grant."
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144-e637; module:VirtoCommerce.Customer=3.1032.0-pr-324-b30b; store=B2B-store; account=storefront buyer
    at: 2026-10-28T14:28:25.245Z
    by: session:56fb99bb
    bodyHash: 2d9af5f2ad99
    who: Lenajava1
    note: "Same shape on a storefront buyer's password grant with storeId and organization_id: 400 invalid_grant, code user_is_temporary_locked_out, one errors[] entry, lockoutSecondsRemaining null, userId null, count 3, while the user record showed a lockoutEnd about 2 minutes out. After lockoutEnd passed, one more attempt with the same (wrong) password returned 400 login_failed."
---
On vcst_qa on 2026-10-21, I sent one form-encoded POST /connect/token (grant_type=password, scope=offline_access) for the platform admin account, which was already temporarily locked out. It returned HTTP 400. The body had error=invalid_grant and code=user_is_temporary_locked_out, with the description "Your account has been temporarily locked. Please try again after some time." It also had an errors[] array with one entry carrying the same code. The response also carried lockoutSecondsRemaining: null and userId: null. So on this build the password-grant refusal uses the same response model that carries lockoutSecondsRemaining on the OTP grant. A client cannot read a countdown from it here, because the value is null even while the account is locked. The count field on the response was 3, which is the token response's parameter count, not an error count. I did not establish why the account was locked, and I did not retry.
