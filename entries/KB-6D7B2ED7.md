---
id: KB-6D7B2ED7
subject: Storefront OTP sign-in form posts to the storefront origin /api/otp/request, which the storefront ingress answers 405; the form shows an inline generic error plus a global server-error toast
plane: experiential
question: What happens when a guest clicks Continue on the storefront OTP email sign-in form?
status: active
appliesTo:
  - axis: feature
    value: otp-email-sign-in
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/otp/request
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T22:34:59.318Z
    by: session:238f2094
    who: kutasinaelena
---
On vcptcore_qa1 (theme 2.59.0-pr-2477-ff59, OTP module 3.1000.0-pr-1-11d6, 2026-10-01) the /sign-in page defaults to the one-time-code view. Clicking Continue with a valid address issues POST /api/otp/request relative to the storefront origin; the storefront ingress returns 405 (text/html, Cloudflare), so the request never reaches the platform. The form stays on the email step and shows an inline alert "Something went wrong. Please try again later." and, at the same time, the app-wide toast "Apologies for the inconvenience. Our server is currently experiencing technical issues..." with a Report a problem button. The same request sent directly to the platform host is answered normally, so the gap is ingress routing of /api/otp, not the module. Empty email shows "This field is required"; john@ and an address with an inner space show "Enter a valid email address, e.g. johndoe@gmail.com" and send nothing.
