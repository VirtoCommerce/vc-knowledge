---
id: KB-E9FD2A2F
subject: The password grant issues a token to an account whose passwordExpired is true; the expiry is enforced downstream, as xAPI errors[].code PasswordExpired
plane: experiential
question: What does POST /connect/token answer for an account whose password has expired (passwordExpired=true), and does a token minted before the expiry keep working?
status: active
appliesTo:
  - axis: grant_type
    value: password
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/platform/security/users/{userName}/resetpassword
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T21:19:29.029Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
On vcptcore_dev (platform 3.1079.0-alpha.13411, Xapi 3.1027.0-alpha.216, Return 3.1005.0-pr-28-fb4f), 2026-10-09:
- An admin POST /api/platform/security/users/{userName}/resetpassword {newPassword, forcePasswordChangeOnNextSignIn:true} answered 200 succeeded:true, and the re-read showed passwordExpired=true.
- POST /connect/token grant_type=password (storeId + organization_id) with the NEW password answered HTTP 200 with an access token (expires_in 1799). The password grant does not refuse an expired password.
- The OLD password answered HTTP 400 invalid_grant, with no error_description.
- The refresh token issued before the reset answered HTTP 400 invalid_grant "The specified refresh token is no longer valid.", so the reset ends the refresh side of the session.
- The access token issued before the reset still authenticated: HTTP 200 on /graphql, no 401.
- Both tokens were refused by Query.returns, Query.organizationReturns and Query.return(id) with HTTP 200, data null and errors[].code PasswordExpired ("This user has their password expired. Please change the password using 'changePassword' command."). Query.returnStatuses still answered.
So an expired account can still sign in, and the expiry shows up as a GraphQL error code on the gated operations, not as a sign-in failure.
