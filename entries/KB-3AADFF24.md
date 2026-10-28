---
id: KB-3AADFF24
subject: A failed password sign-in on /sign-in sends exactly one GA4 login hit with ep.success=false and ep.errors naming the token error code, from an anonymous session
plane: experiential
question: What GA4 collect request does a failed password sign-in on the storefront /sign-in page send?
questions:
  - text: Does a wrong password on /sign-in fire en=login with success=false?
  - text: Is a locked-out sign-in reported to GA, and with which error?
status: active
appliesTo:
  - axis: integration
    value: google-analytics-4
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-28T18:14:32.941Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
---
On a fresh /sign-in page (password mode), one submit with a wrong password produced POST /connect/token 400 (code login_failed) and exactly one GA4 /g/collect hit with en=login, ep.success=false, ep.method=password and ep.errors="login_failed: Login attempt failed. Please check your credentials." The session's user properties (sent on the page_view hit) were up.organization_id empty, up.contact_id empty, up.session_kind=anonymous. A further submit that hit a temporarily locked account produced a second, separate en=login hit with ep.success=false and ep.errors starting user_is_temporary_locked_out, while the form showed the same generic 'Login attempt failed. Check your credentials' alert in both cases. One hit per attempt, never a success=true hit.
