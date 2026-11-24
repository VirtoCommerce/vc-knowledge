---
id: KB-EFE3BD7D
subject: In dark mode the storefront page-builder slider block renders white heading text on a light grey background (1.33:1)
plane: experiential
question: Is the heading text of a page-builder slider block readable on the storefront in dark mode?
status: active
appliesTo:
  - axis: component
    value: page-builder slider-block
  - axis: scheme
    value: dark
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /designer-preview
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676-6b5e-6b5e8717; store=B2B-store
    defect: suspected
    at: 2026-11-19T13:43:58.139Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676-6b5e-6b5e8717; store=B2B-store; browser=firefox; role=guest
    at: 2026-11-19T18:04:42.956Z
    by: session:51ac2fe3
    bodyHash: 41cd705d61b7
    defect: suspected
    who: Lenajava1
    note: "Re-observed 2026-11-19 on the same draft page route, Firefox, 1920 px, anonymous: .slider-block bg-neutral-800 py-10 lg:py-24 computes to color(srgb 0.8667 0.8745 0.898) = #dddfe5 in html.dark while its headings (48 px \"Virto Page\", 24 px \"Saveguard\", \"Security First\") stay rgb(255,255,255): 1.33:1. Mechanism: --color-neutral-800 is #dddfe5 in dark and #262626 in light (the neutral ramp inverts), the heading colour is a literal white. In light the same block is #262626 with white text (about 15.9:1). The page is still anonymously readable via /designer-preview?pageId (VCST-6942 class)."
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676-6b5e-6b5e8717; store=B2B-store; browser=firefox; role=guest
    at: 2026-11-20T07:26:03.207Z
    by: session:51ac2fe3
    bodyHash: 41cd705d61b7
    defect: VCST-6949
    who: Lenajava1
    note: "Re-measured 1.33:1 at 1920 and 384 on theme 2.63.0-pr-2676 and on flag-off 2.63.0-pr-2630. Cause: slider.vue literal `color: white` on `.bg-neutral-800` + dark presets inverting --color-neutral-800; pre-existing since the dark presets (2026-03), also in stable bundles v15/v16; nine more static-content blocks share the rule."
  - method: observation
    deployment: vcst_qa
    build: 54b87331b2bf6e07
    conditions: theme=2.63.0-pr-2676-f03c-f03c41eb; store=B2B-store; browser=chromium; role=guest
    at: 2026-11-24T14:22:25.051Z
    by: session:c6b4c92c
    bodyHash: 41cd705d61b7
    who: Lenajava1
    note: "Re-measured 2026-11-24 in Chromium, 1920 and 384 px, html.dark (Theme auto + prefers-color-scheme dark): .slider-block bg #dddfe5, headings #ffffff, 1.33:1. Same 1.33:1 on /test-demo-landing for text-block (24/18 px) and call-to-action-block heading (30 px bold). The PR build theme 2.63.0-pr-2688-81d1 (vc-frontend PR #2688, not yet a release) renders the same headings #0a090b on #dddfe5 = 14.91:1."
---
On vcst_qa (theme 2.63.0-pr-2676, store B2B-store, Firefox, 1920 px and 384 px), GET /designer-preview?pageId=<draft page id> with html.dark (header Theme toggle auto -> dark): the page-builder slider block (div.slider-block with utility classes bg-neutral-800 py-10 lg:py-24) has computed background color(srgb 0.8667 0.8745 0.898) = #dddfe5 while its heading text (Virto Page, SAVEGUARD, Security First, 24-48 px bold) is rgb(255,255,255): contrast 1.33:1, below the 3:1 large-text minimum. In light mode the same block is dark grey with white text and is readable. The same block shows the same colours inside the Site Builder designer preview iframe in the dark scheme, so the preview shows what the storefront shows.
