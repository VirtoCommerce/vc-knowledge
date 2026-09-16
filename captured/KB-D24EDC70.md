---
id: KB-D24EDC70
subject: what refuses a never-registered account at sign-in was not established, and here is where the looking stopped
plane: experiential
question: does a security account with status PendingApproval or emailConfirmed false block storefront sign-in
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/platform/security/users/{userName}
evidence:
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-15T17:40:00.000Z
    by: round3-arm-C
---

Nobody has shown what refuses it. Two arms of round three went after this independently and both stopped in the same place, which is why it is worth recording as a gap rather than leaving it to be re-searched. What IS established about such an account: status PendingApproval, emailConfirmed false, zero roles, lastLoginDate null, and the store carrying Stores.EmailVerificationEnabled and Stores.EmailVerificationRequired both True - four independent fields agreeing it was never registered. What is NOT established is which check does the refusing. ContactSignInValidator's email-verification branch does not refuse a login at all: it substitutes a clearer error message on a sign-in that has ALREADY failed as locked out with contact.Status Locked, and a never-registered account is not locked out, so that branch never fires for it. No code refusing on user.Status == PendingApproval was found anywhere, searched for deliberately. The likely gate is ASP.NET Identity's SignIn.RequireConfirmedEmail, which lives in server configuration and is exposed by no endpoint either arm could read. So do not write PendingApproval blocks sign-in in a test expectation: the state is certain, the mechanism is not, and settling it needs either the server configuration or an actual sign-in attempt, which writes to a lockout counter.
