---
id: KB-7B4CAFF7
subject: The storefront registers its FCM service worker only after sign-in, under scope /firebase-cloud-messaging-push-scope, with a versioned file name that changes with the theme build (v1.12 on theme 2.62.0-pr-2629, v1.13 on 2.62.0-pr-2628)
plane: experiential
question: Which FCM service worker file does the storefront register, under which scope, and when?
questions:
  - text: When does the storefront register its firebase messaging service worker?
  - text: What is the scope of the storefront push-messages service worker registration?
  - text: Is the fcm-service-worker file name versioned per theme build?
status: active
appliesTo:
  - axis: auth
    value: signed-in
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /firebase-cloud-messaging-push-scope
  - coordinate: GET /fcm-service-worker-v1.13.js
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: theme=2.62.0-pr-2628; browser=firefox-playwright
    at: 2026-11-11T13:50:33.967Z
    by: session:7773ae1e
    who: Lenajava1
---
2026-11-11, store B2B-store, Firefox lane. Anonymous home: navigator.serviceWorker.getRegistrations() returned none. After password sign-in of a buyer the page held one registration, scope {storefront}/firebase-cloud-messaging-push-scope, active script {storefront}/fcm-service-worker-v1.13.js on theme 2.62.0-pr-2628 (a frontend-only build proxied to vcst_qa) and {storefront}/fcm-service-worker-v1.12.js on the deployed theme 2.62.0-pr-2629. GET /fcm-service-worker-v1.13.js answered 200 on the 2628 build and 404 on the 2629 build; the 2628 file declares VERSION = "12.19.0" and importScripts the gstatic firebase-app-compat and firebase-messaging-compat scripts of that version (both 200). The file name is the SERVICE_WORKER_PATH constant of client-app/modules/push-messages/constants, so a check must read it from the build under test rather than hard-code it. In the Playwright Firefox lane pushManager.getSubscription() throws "Error retrieving push subscription", so no FCM token (saved-fcm-token localStorage key) is obtained there.
