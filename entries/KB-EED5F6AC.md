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
---
vcst_qa 2026-10-09, theme vc-frontend-next 3.0.0-alpha.2685: during an OTP sign-in then Logout from the account menu, the browser fetched GET {storefront}/firebase-messaging-sw.js as a script at the moment of logout; the storefront returned 404 (text/html), and the console logged one error "A bad HTTP response code (404) was received when fetching the script." The request does not appear in the page's own network list (it is a service-worker script fetch) but is in the HAR. A direct GET of the same path also returns 404. Sign-out itself completed normally (redirect to /sign-in?returnUrl=/account/dashboard, header shows Sign in). Whether this is a missing web-push (Firebase) configuration on this deployment or a theme packaging gap was not determined.
