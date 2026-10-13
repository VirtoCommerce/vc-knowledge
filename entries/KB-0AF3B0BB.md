---
id: KB-0AF3B0BB
subject: "Home-page CLS of about 0.5 at 1920 predates theme PR 2543: the pre-PR build (pr-2485) measures 0.488 signed in, with the same Builder.io block and footer shift geometry as the PR build (0.551)"
plane: experiential
question: Is the high CLS on the storefront home page pre-existing on an older theme build, or introduced by a newer one, and what shifts?
status: active
appliesTo:
  - axis: auth
    value: signed-in
  - axis: page
    value: home
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 1920
anchors:
  - coordinate: Query.slugInfo
evidence:
  - method: observation
    deployment: vcptcore_qa
    conditions: theme=2.60.0-pr-2485-f6cd-f6cd22b2; comparison-theme=2.60.0-pr-2543-0127-01276486 on vcst_qa; viewport=1920x1080; browser=chromium; auth=signed-in buyer
    at: 2026-10-13T17:26:07.796Z
    by: session:aa76d86f
    who: Lenajava1
---
Measured 3 loads each at 1920x1080 in Chromium, signed in as the same buyer on both stands, with a layout-shift PerformanceObserver (buffered, hadRecentInput excluded, window loadEventEnd+5s). The home page is served by the catch-all matcher route; the DOM path div.matcher > div.slug-content was observed. On the pre-PR build (theme 2.60.0-pr-2485) the signed-in home renders Builder.io content and measures CLS 0.477/0.477/0.510 (mean 0.488). Its top shift (0.347) is from two .builder-block nodes (dy -625/dh -455 and dy -191/dh -437), followed by footer#footer at 0.13-0.15 (dy -816 to -898). On the PR build (theme 2.60.0-pr-2543, another stand) the signed-in home measures 0.551 x3: builder blocks 0.406 with identical dy/dh, and the footer at 0.144 (dy -816). The footer first sits at y of about 816 (the header plus the main-layout__content min-h-[60vh]) while slug-content renders nothing until the page is resolved. It is then pushed below the fold, so the shift exists independently of the product-list data attributes the PR adds. The absolute difference follows the stand-specific Builder.io content.
