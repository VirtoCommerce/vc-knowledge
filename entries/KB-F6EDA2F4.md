---
id: KB-F6EDA2F4
subject: the Vendor Portal authenticates by HttpOnly cookies, so local-storage tampering does not end the session
plane: experiential
question: How is a Vendor Portal session at /apps/vendor-portal authenticated, and can session expiry be triggered client-side?
status: active
appliesTo:
  - axis: surface
    value: vendor-portal-ui
anchors:
  - coordinate: /apps/vendor-portal
  - coordinate: /connect/authorize
evidence:
  - method: observation
    deployment: vcmp_dev
    at: 2026-09-25T11:19:02.414Z
    by: session:memimpor
    who: Lenajava1
---
The Vendor Portal's API calls are authenticated by HttpOnly identity cookies, not only by the bearer data it keeps in local storage: document.cookie reads empty, corrupting the local-storage auth entry changes nothing and calls still return 200, and only clearing the cookies produces 401. No client-side change produces a 403, and /Account/Login, /account/login and /Account/AccessDenied return 404 while /connect/authorize returns 400, so session-expiry paths cannot be reached naturally. The version shown in the login footer is the app's own version; the vc-shell framework version appears in a console banner, and the two differing is normal.
