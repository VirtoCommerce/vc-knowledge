---
id: KB-1E03BE5A
subject: The storefront Barcode scan control sits inside the desktop header search input at 1920 px, but at 390 px it is reachable only after "Toggle search bar"
plane: experiential
question: where is the Barcode scan entry point in the storefront header at desktop vs mobile width
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 390,1920
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
