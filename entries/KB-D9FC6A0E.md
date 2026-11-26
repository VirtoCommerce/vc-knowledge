---
id: KB-D9FC6A0E
subject: Storefront breadcrumb links take the preset's color_link, else the kit fallback accent-600
plane: experiential
question: "storefront breadcrumbs link colour: which CSS variable drives VcBreadcrumbs link color, and what does it resolve to per theme preset?"
questions:
  - text: Why are breadcrumb links teal instead of the link colour on a preset?
  - text: What colour are VcBreadcrumbs links in Storybook vs the storefront?
  - text: Does --vc-breadcrumbs-link-color follow color_link?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /catalog
  - coordinate: /product/{id}
evidence:
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    conditions: theme=2.63.0-pr-2674-fc38; setting:ThemePreset=coffee|red
    at: 2026-11-18T20:06:32.036Z
    by: session:feee22e7
---
On the storefront the VcBreadcrumbs link colour reads --vc-breadcrumbs-link-color, which the app maps to --link-color. A preset that defines color_link (Red: #2563eb) shows that colour on PLP and PDP. A preset without color_link (Coffee) falls back to the kit default accent-600 (Coffee: #38647b). Since vc-frontend PR #2674 (theme 2.63.0-pr-2674), the kit itself sets var(--vc-breadcrumbs-link-color, accent-600). In Storybook nothing sets the variable, so links always show accent-600 there, and differ from color_link on black-gold, watermelon and the dark variants of paprika and purple-pink. Before that PR, Storybook showed neutral grey.
