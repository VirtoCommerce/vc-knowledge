---
id: KB-38B0EAE3
subject: A second OTP request for the same email within a short window emails the same 6-digit code again rather than a new one
plane: experiential
question: Does a repeated POST /api/otp/request for the same email issue a new OTP code or re-send the previous one?
questions:
  - text: Is the OTP code regenerated on every Continue on the storefront sign-in?
  - text: Why do two OtpSignInEmailNotification rows carry the same code?
status: active
appliesTo:
  - axis: feature
    value: otp-sign-in
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/otp/request
  - coordinate: POST /connect/token
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 023779f193646d5e
    conditions: theme=3.2.0-pr-19-4507-4507f3ae; store=B2B-store
    at: 2026-11-02T19:54:00.226Z
    by: session:75d360f6
    who: Aleksandra-Mitricheva
---
On the B2B store, a buyer's OTP sign-in was requested twice about 1 min 45 s apart (19:42:43Z and 19:44:28Z; the first by another concurrent session, the second by Continue on /sign-in). The notification journal held two OtpSignInEmailNotification rows for that email, and both bodies carried the same 6-digit code. The code typed after the second request signed in (POST /connect/token grant_type=otp_email 200). A wrong code before it answered 400 and showed "That code didn't work." on the code step. So, within this window, a repeat request re-sends the still-valid code rather than issuing a new one. Not established: the code's lifetime, and whether the first session had already consumed the code (that would touch the reuse defect VCST-6490).
