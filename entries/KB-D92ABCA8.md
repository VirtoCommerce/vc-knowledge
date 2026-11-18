---
id: KB-D92ABCA8
subject: "A refused otp_email grant because the store's OTP sign-in is off is written to the sign-in log as signInType OTP, succeeded false, failureReason OtpDisabled, but asynchronously: a search issued about 1 s after the 400 returned no row, the same search seconds later returned it"
plane: experiential
question: Does the sign-in log record an OTP grant refused because OTP sign-in is disabled for the store, and is it searchable immediately?
questions:
  - text: What failureReason does the sign-in log show for an otp_email grant when OtpSignIn.Enabled is off?
  - text: Is the platform sign-in log written synchronously with /connect/token?
status: active
appliesTo:
  - axis: grant
    value: otp_email
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/platform/security/sign-in-log/search
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: platform=3.1099.0; module:VirtoCommerce.OTP=3.1002.0-pr-4-83d4; setting:OtpSignIn.Enabled=false
    at: 2026-11-18T15:39:29.769Z
    by: session:6da8340c
    who: Lenajava1
---
On vcst (platform 3.1099.0, VirtoCommerce.OTP 3.1002.0-pr-4-83d4), after POST /connect/token grant_type=otp_email answered 400 code otp_disabled (row createdDate 15:37:54.64Z), POST /api/platform/security/sign-in-log/search {userName:<email>, startDate:<time before the code request>, take:20, sort:"createdDate:desc"} issued right after (an admin password token was fetched in between, ~1 s) returned 200 totalCount 0. The identical body sent again about a minute later returned totalCount 1: signInType "OTP", succeeded false, failureReason "OtpDisabled". Other failure reasons seen for the same user on the same day: InvalidCode. A case asserting the row should poll the search briefly rather than read it once.
