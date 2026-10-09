---
id: KB-EED5F6AC
subject: On vcst_qa the storefront requests /firebase-messaging-sw.js on sign-out and the storefront answers 404, logging a console error "A bad HTTP response code (404) was received when fetching the script."
plane: experiential
question: Does the storefront web-push service worker script /firebase-messaging-sw.js resolve on vcst_qa, and what shows up when it does not?
status: active
appliesTo:
  - axis: feature
    value: web-push-notifications
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /firebase-messaging-sw.js
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T13:56:30.833Z
    by: session:df9d131f
    who: Lenajava1
---
vcst_qa 2026-10-09, theme vc-frontend-next 3.0.0-alpha.2685: during an OTP sign-in then Logout from the account menu, the browser fetched GET {storefront}/firebase-messaging-sw.js as a script at the moment of logout; the storefront returned 404 (text/html), and the console logged one error "A bad HTTP response code (404) was received when fetching the script." The request does not appear in the page's own network list (it is a service-worker script fetch) but is in the HAR. A direct GET of the same path also returns 404. Sign-out itself completed normally (redirect to /sign-in?returnUrl=/account/dashboard, header shows Sign in). Whether this is a missing web-push (Firebase) configuration on this deployment or a theme packaging gap was not determined.
