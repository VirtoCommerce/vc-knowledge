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
    build: a6e179f178104063
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: theme=2.60.0-alpha.2579; platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; setting:PushMessages.FCM.Enable=true
    at: 2026-10-15T13:09:10.250Z
    by: session:6ee2ca49
    bodyHash: 6a343caecf3f
    who: Lenajava1
    note: "Re-seen 2026-10-15 on theme 2.60.0-alpha.2579 (module federation enabled): GET /firebase-messaging-sw.js 404 text/html with a Service-Worker request header, 8 times in one session, each when the account menu was opened before Logout (rep, maintainer, buyer accounts alike); same console error. Sign-out still completed."
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: theme=2.60.0-alpha.2579; platform=3.1083.0
    at: 2026-10-15T13:42:02.019Z
    by: session:6ee2ca49
    bodyHash: 6a343caecf3f
    who: Lenajava1
    note: "theme 2.60.0-alpha.2579: same console error 'A bad HTTP response code (404) was received when fetching the script.' logged at password-sign-in Logout from the account menu, twice (two users); the request is absent from the page network list, consistent with a service-worker script fetch. Script URL itself not re-read from a HAR this time."
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=vc-frontend-next 3.1.0-pr-11-d67b0e0a; notification-permission=not-granted (automation browser); signin=password
    at: 2026-10-20T15:57:22.907Z
    by: session:d992339e
    bodyHash: 6a343caecf3f
    who: Lenajava1
    note: "Re-seen 2026-10-20, 2/2 sign-in then Logout cycles (once on /compare, once on a configurable PDP); guest page loads of the same two pages logged nothing (0/3), and signed-in page loads logged nothing either. HAR: at sign-in the theme registers GET /fcm-service-worker-v1.12.js (200, Service-Worker: script); at Logout the browser fetches GET /firebase-messaging-sw.js (404 text/html, Service-Worker: script), then POST /revoke/token 200 and sign-out completes. So the error is bound to Logout, not to the page it happens on. Source (read, not observed): /firebase-messaging-sw.js is the Firebase JS SDK's DEFAULT_SW_PATH; the theme's logout handler calls deleteToken(messaging), and the SDK registers that default worker when messaging.swRegistration was never set -- which is the case whenever getToken threw permission-blocked at sign-in because notification permission was not granted."
---
vcst_qa 2026-10-09, theme vc-frontend-next 3.0.0-alpha.2685: during an OTP sign-in then Logout from the account menu, the browser fetched GET {storefront}/firebase-messaging-sw.js as a script at the moment of logout; the storefront returned 404 (text/html), and the console logged one error "A bad HTTP response code (404) was received when fetching the script." The request does not appear in the page's own network list (it is a service-worker script fetch) but is in the HAR. A direct GET of the same path also returns 404. Sign-out itself completed normally (redirect to /sign-in?returnUrl=/account/dashboard, header shows Sign in). Whether this is a missing web-push (Firebase) configuration on this deployment or a theme packaging gap was not determined.
