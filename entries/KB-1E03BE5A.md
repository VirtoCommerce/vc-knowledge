---
id: KB-1E03BE5A
subject: The storefront Barcode scan control sits inside the desktop header search input at 1920 px, but at 390 px it is reachable only after "Toggle search bar"
plane: experiential
question: where is the Barcode scan entry point in the storefront header at desktop vs mobile width
questions:
  - text: Where is the barcode scan button in the storefront header on desktop and on mobile?
  - text: search-bar.vue mobile-search-bar.vue Barcode scan control location desktop 1920 vs 390
  - text: Why can't I find the Barcode scan control in the header at 390 px width?
  - text: Does the barcode scanner appear only after tapping Toggle search bar on mobile?
  - text: Is the Barcode scan dialog opened directly from the desktop header search input at 1920x1080?
concepts:
  - id: barcode-scanner
  - id: mobile-layout
  - id: site-header
status: active
appliesTo:
  - axis: viewport
    value: 390,1920
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: client-app/shared/layout/components/header/_internal/search-bar/search-bar.vue
  - coordinate: client-app/shared/layout/components/header/_internal/mobile-search-bar.vue
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T16:47:00.136Z
    by: session:2d9f6d8e
    who: Lenajava1
---
On 2026-10-05 (vc-frontend 2.59 builds, anonymous, Chromium): at 1920x1080 the Barcode scan control is rendered inside the desktop header search input and opens the Barcode scan dialog directly. At 390x844 the header shows a "Toggle search bar" button; the Barcode scan control appears only in the opened mobile search overlay. An earlier note that the entry point was not found at 1920 px was wrong.
