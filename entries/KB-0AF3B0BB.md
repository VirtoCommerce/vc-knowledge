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
  - method: observation
    deployment: vcst_qa
    build: 16bf801dd4d62d5a
    conditions: theme=2.62.0-pr-2600-a4f2-a4f29534; comparison=local build dev@4ce61d0c; viewport=1920x1080; auth=anonymous
    at: 2026-11-02T08:32:44.802Z
    by: session:a1013b77
    bodyHash: 27d37f731e3f
    who: Lenajava1
    note: "2026-11-02, anonymous, 1920x1080 Chromium, observer installed before navigation: home CLS 0.25 on vcst theme 2.62.0-pr-2600 (classic), 0.46 on a local build of base dev@4ce61d0c (classic) and 0.43-0.46 on a local soft-design build of the same PR, all proxied to the same backend. Same shift geometry on all three: footer#footer dy -816 plus div.features-block (dy +450..+719 then -887), all before 2.1 s. The web-font swap adds only 0.001-0.002 even with the font stylesheet held 6 s."
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534 soft (local) vs dev@4ce61d0c classic-BASE (local), same vcst backend; browser=chromium 1920x1080; auth=signed-in buyer; http-cache=warm
    at: 2026-11-02T09:46:17.295Z
    by: session:be9f7d04
    bodyHash: 27d37f731e3f
    who: Lenajava1
    note: "Buffered layout-shift observer, signed in: home CLS 0.296 soft vs 0.277 classic-BASE, both dominated by FOOTER#footer (~0.277 at ~1.7 s) after content resolves; PDP 0.149 then 0.141 soft vs 0.141 classic-BASE (svg/footer 0.117 + footer 0.023). No shift coincides with the font stylesheet arrival (~0.2 s)."
---
Measured 3 loads each at 1920x1080 in Chromium, signed in as the same buyer on both stands, with a layout-shift PerformanceObserver (buffered, hadRecentInput excluded, window loadEventEnd+5s). The home page is served by the catch-all matcher route; the DOM path div.matcher > div.slug-content was observed. On the pre-PR build (theme 2.60.0-pr-2485) the signed-in home renders Builder.io content and measures CLS 0.477/0.477/0.510 (mean 0.488). Its top shift (0.347) is from two .builder-block nodes (dy -625/dh -455 and dy -191/dh -437), followed by footer#footer at 0.13-0.15 (dy -816 to -898). On the PR build (theme 2.60.0-pr-2543, another stand) the signed-in home measures 0.551 x3: builder blocks 0.406 with identical dy/dh, and the footer at 0.144 (dy -816). The footer first sits at y of about 816 (the header plus the main-layout__content min-h-[60vh]) while slug-content renders nothing until the page is resolved. It is then pushed below the fold, so the shift exists independently of the product-list data attributes the PR adds. The absolute difference follows the stand-specific Builder.io content.
