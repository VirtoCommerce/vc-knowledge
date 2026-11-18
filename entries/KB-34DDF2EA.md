---
id: KB-34DDF2EA
subject: In the soft design, VcPagination page buttons are 32px round pills with a 1px ring and a 2px gap; in dark the ring is the light ink at 16% and it renders in Firefox 156
plane: experiential
question: What does the soft-design VcPagination page button compute to (radius, ring, gap, hover, current page, ellipsis) in light and dark, and does the dark relative-colour ring render in Firefox?
questions:
  - text: Does rgb(from var(--color-additional-950) r g b / 0.16) render as the soft pagination dark ring in Firefox?
  - text: Is the soft VcPagination ellipsis greyed to neutral-400?
  - text: What is the current page ring colour in soft VcPagination?
status: active
appliesTo:
  - axis: component
    value: vcpagination
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /client-app/designs/soft/_tokens.scss
  - coordinate: /client-app/ui-kit/components/organisms/pagination/vc-pagination.vue
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: theme=2.63.0-pr-2617-a143-a1435e3f; surface=storybook; browser=firefox-156; setting:design=soft|classic; preset=red|coffee
    at: 2026-11-18T15:23:57.587Z
    by: session:8bd71225
    who: Lenajava1
---
QA Storybook of theme build 2.63.0-pr-2617-a143 (story components-organisms-vcpagination--page-6, args pages=147 page=6), Firefox 156, getComputedStyle on .vc-pagination__page. Soft, light (red and coffee): page buttons 32x32, border-radius 9999px, border 1px solid neutral-200 (#e5e5e5 in red), gap between pages 2px, Inter 12px/700; hover ring primary-200 on a primary-50 fill; current page ring transparent over the primary-500 fill (visually the fill colour); the ellipsis has border 0 and no colour of its own - it inherits the surrounding text colour (black in Storybook light), it is NOT neutral-400. Soft, dark: the ring computes to the additional-950 ink at alpha 0.16 (red: color(srgb 0.97 0.97 0.97 / 0.16) on #0a0a0a, about 1.5:1; coffee: the warm ink at 0.16 on #110f0e) - Firefox supports the relative colour syntax (CSS.supports true) and the ring is visible on screen; hover ring is primary-500. Classic in the same build: radius 4px, border-width 0, gap normal (0), Lato; the latent border-color of the current page equals the rest pages' default border colour, not the fill. Pages with 5-digit numbers grow wider than 32px (Inter proportional digits, 47-50px). With 9 items and compact on, at 288px available width soft wraps the last page onto its own row while classic fits all 9 on one row (9x32 + 8x2 = 304px).
