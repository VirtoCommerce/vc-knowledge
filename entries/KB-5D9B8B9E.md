---
id: KB-5D9B8B9E
subject: "Guest home page at 1280x900 has CLS ~0.173: the footer jumps ~718px when the async home page replaces the Matcher route loader"
plane: experiential
question: Does the storefront home page shift layout on load, and which element causes it?
questions:
  - text: What is the CLS of the guest home page at 1280x900?
  - text: Why does the footer move on the storefront home page load?
  - text: Does the Matcher loader reserve space for the home page content?
status: active
appliesTo:
  - axis: auth
    value: guest
  - axis: page
    value: home
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 1280x900
anchors:
  - coordinate: /home.vue
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: theme=2.59.0; store=B2B-store; throttle=fast3g; browser=chromium
    defect: suspected
    at: 2026-11-05T07:26:01.443Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
---
Fresh guest load of the storefront home page (site root) at 1280x900, Fast-3G, layout-shift observer installed pre-paint (deterministic layout runner, 6 runs): CLS 0.1731-0.1733, 96% from footer#footer (impact 0.166, moved 718px). Visually: the first paint shows header, an empty main with a centred VcLoaderOverlay (main has min-h 60vh, ~540px) and the footer at y~718, with no scrollbar. Then the home body (sign-in hero, features, subscribe; ~1470px) mounts and pushes the footer below the fold. In vc-frontend 2.59.0 the root is served by the catch-all Matcher route: it shows the loader while its previewers settle, then the internal previewer mounts the lazily imported pages/home.vue, and no skeleton or height is reserved. No image element is among the contributors. All network 2xx, console clean.
