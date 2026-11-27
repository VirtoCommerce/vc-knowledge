---
id: KB-B80BC7C6
subject: "After sign-in, an organization's own white-labeling themePresetName replaces the store preset on every page (home, /catalog, /cart): html data-preset, --color-primary-500 and solid-primary button backgrounds all follow the organization preset"
plane: experiential
question: Does the organization's theme preset apply across storefront pages after a member signs in, or only on the home page?
questions:
  - text: Which preset does /catalog use for a signed-in member whose organization has its own white-labeling preset?
  - text: Does the solid primary button colour follow the organization preset after sign-in?
  - text: Is --color-primary-500 the same on home, catalog and cart for an organization member?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: desktop
anchors:
  - coordinate: Query.pageContext
  - coordinate: /catalog
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; theme=2.63.0; store=B2B-store; browser=edge
    at: 2026-11-27T14:10:41.392Z
    by: session:d2ee4088
    who: Lenajava1
---
vcst_qa 2026-11-27, theme 2.63.0, Edge 1920x1080, store B2B-store with an enabled store white-labeling record (preset Mercury). As a guest the home page has <html data-preset="mercury"> and --color-primary-500 #f99e24. After signing in (password form) as a member of an organization whose enabled white-labeling record has themePresetName Coffee, <html data-preset> is "coffee" and --color-primary-500 is #996c5a on /, /catalog and /cart alike, and every visible .vc-button--solid--primary has background rgb(153,108,90), i.e. the variable value. A second organization with a theme-only record (preset black-gold, no logo or lists) gives data-preset "black-gold" and --color-primary-500 #1d1f22. No preset file is requested; the switch happens on the page context reload after sign-in.
