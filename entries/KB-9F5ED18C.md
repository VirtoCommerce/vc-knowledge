---
id: KB-9F5ED18C
subject: With the paprika light preset, white text on solid-primary buttons and primary badges is #ffffff on #e5451c = 4.04:1, below the 4.5:1 WCAG 1.4.3 floor; the paprika dark preset uses black on #e17853 = 7.01:1
plane: experiential
question: Do solid-primary buttons and the header cart badge meet WCAG 1.4.3 contrast in the paprika preset, light and dark?
status: active
appliesTo:
  - axis: mode
    value: dark
  - axis: mode
    value: light
  - axis: preset
    value: paprika
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: /sign-in
  - coordinate: /product/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T11:30:38.046Z
    by: session:df9d131f
    who: Lenajava1
    build: a6e179f178104063
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; role=guest; browser=chromium; viewport=1920x1080; mode=light
    at: 2026-10-20T14:22:49.182Z
    by: session:d992339e
    bodyHash: 4a2b7b67ac71
    who: Lenajava1
    note: "Re-observed on theme 3.1.0-pr-11-d67b, guest, 1920 light: header Sign in white #ffffff on --color-primary-500 #e5451c, axe-core 4.10.2 color-contrast 4.03:1 (the only violation on /compare and on a file-upload configurable PDP); the product-card quantity stepper Increase button (28x28) is the same 4.04:1 pair, icon-only so it falls under 1.4.11 and passes. Outline-primary text (Add to cart on a configurable PDP) is #b6380f on #ffffff = 5.90:1, border #e5451c 3.98:1."
---
Guest, 1920px, getComputedStyle with alpha compositing up the ancestor chain, plus axe-core 4.12.1 color-contrast scoped to .vc-button--solid--primary and .vc-badge. Light: --color-primary-500 #e5451c, text --color-additional-50 #ffffff. 'Sign in' (14px bold), 'Browse catalog' on the home page (16px bold), the clear-cart dialog primary action (16px bold) and the header cart badge count (12px bold, .vc-badge--solid--primary) all measure 4.04:1. axe reports 4.03 as a violation on the buttons, and as 'incomplete' (content too short) on the badge. Hover darkens the fill to about #c33b18 = 5.30:1, which passes. Icon-only solid-primary buttons (header Search, quantity stepper) have the same 4.04 pair, but they fall under 1.4.11 (3:1) and pass. Disabled solid-primary renders #6b5f55 on #e7ddce (4.61) and is exempt anyway. Dark (Appearance = Dark): --color-primary-500 #e17853 with black text = 7.01:1, and axe finds 0 violations. Seen identically on vcst_qa (vc-frontend-next 3.0.0-alpha.2685) and on virtostart (2.59.0-pr-2494, the paprika demo PR build). The paprika preset's own color_primary_600 #b6380f would give 5.90:1 with white.
