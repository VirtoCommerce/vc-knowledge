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
---
On vcst_qa (theme 2.63.0-pr-2676, store B2B-store, Firefox, 1920 px and 384 px), GET /designer-preview?pageId=<draft page id> with html.dark (header Theme toggle auto -> dark): the page-builder slider block (div.slider-block with utility classes bg-neutral-800 py-10 lg:py-24) has computed background color(srgb 0.8667 0.8745 0.898) = #dddfe5 while its heading text (Virto Page, SAVEGUARD, Security First, 24-48 px bold) is rgb(255,255,255): contrast 1.33:1, below the 3:1 large-text minimum. In light mode the same block is dark grey with white text and is readable. The same block shows the same colours inside the Site Builder designer preview iframe in the dark scheme, so the preview shows what the storefront shows.
