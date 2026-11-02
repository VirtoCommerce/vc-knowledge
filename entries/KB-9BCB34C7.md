---
id: KB-9BCB34C7
subject: Design soft in vc-frontend PR 2600 sets only --vc-radius (1rem), --vc-base-font-family (Inter) and --vc-heading-font-family (Geologica), plus footer column titles text-transform none; every scale token (--vc-radius-*, --vc-shadow-*, --vc-font-size-*, --vc-line-height-*, --vc-font-weight-*, --vc-letter-spacing-*) and heading role token stays unset, so it falls back to the classic value
plane: experiential
question: With design soft on vc-frontend, which shape and typography tokens take soft values and which keep classic values?
questions:
  - text: Does the soft design change shadows, font sizes, weights or letter spacing?
  - text: Which --vc-* tokens does client-app/designs/soft set?
  - text: Do Tailwind rounded-* utilities change under design soft?
  - text: Does soft have separate dark-mode shape or typography values?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend
anchors:
  - coordinate: /designs/soft/_tokens.scss
  - coordinate: /designs/soft/blocks/footer-links.scss
evidence:
  - method: source
    deployment: vc-frontend@a4f29534
    conditions: theme=vc-frontend PR 2600 @ a4f29534; setting:design=soft
    at: 2026-11-02T09:06:55.025Z
    by: session:be9f7d04
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 0940c86d8a29dd8a
    conditions: theme=vc-frontend PR 2600 @ a4f29534 local build; setting:design=soft; browser=firefox 1920x1080
    at: 2026-11-02T09:26:40.378Z
    by: session:be9f7d04
    bodyHash: 61d925a4da52
    who: Lenajava1
    note: "Observed live: home (anonymous) soft-PR vs classic-BASE, light and dark, 1177-1196 element pairs: font-size, font-weight, letter-spacing, font-style and box-shadow 0 differences; only font family, --vc-radius consumers (8->16, 6->14), footer title case and 13 inputs' line-height (19->20px, Firefox computes line-height:normal from font metrics) differ."
  - method: observation
    deployment: vcst_qa
    build: 0940c86d8a29dd8a
    conditions: theme=vc-frontend PR 2600 @ a4f29534; setting:design=soft (local build); browser=chromium 1920x1080; auth=signed-in buyer
    at: 2026-11-02T09:46:05.440Z
    by: session:be9f7d04
    bodyHash: 61d925a4da52
    who: Lenajava1
    note: "Chromium, signed in, light + dark: the loaded CSSOM holds exactly two design-scoped rules (html[data-design=soft] in layer theme: --vc-radius 1rem, --vc-base-font-family, --vc-heading-font-family; footer-links__title text-transform none in layer vc.design) and no rule anywhere sets a --vc-radius-*/--vc-shadow-*/--vc-font-size-*/--vc-line-height-*/--vc-font-weight-*/--vc-letter-spacing-*/--vc-heading-(weight/style/transform/spacing) token, light or .dark. On home, /catalog, PDP, /cart, /account/orders: .rounded 4px, .shadow-md 0 4px 6px 0 / 0 2px 4px -2px, .text-sm 14/18px, .font-bold 700, .tracking-wide 0.3px, h1-h3 weight/style/case unchanged = classic-BASE; only .vc-button 6->14px, .vc-widget 8->16px, body Inter, h1-h3 Geologica differ. Dark: shadow geometry identical, colour only, same as classic-BASE dark."
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534; setting:design=soft (local build); browser=chromium 1920x1080
    at: 2026-11-02T09:54:55.454Z
    by: session:9e59a9c2
    bodyHash: 61d925a4da52
    who: Lenajava1
    note: "Chromium, html root computed custom props light and .dark: only --vc-radius 1rem, --vc-base-font-family, --vc-heading-font-family; text-sm 14/18, font-bold 700, tracking 0.3px, h1 30/45/700/0.75px identical to classic-BASE."
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534 local build; setting:design=soft; baseline=vc-frontend dev@4ce61d0c local build, design classic; browser=Edge (Chromium) 1280 + 375, light and dark; pages=home(Builder), /cart, /account/orders signed in
    at: 2026-11-02T10:01:30.606Z
    by: session:be9f7d04
    bodyHash: 61d925a4da52
    who: Lenajava1
    note: "Chromium/Edge, element-paired by DOM path, soft vs classic-BASE home light 1461 pairs / dark 1443 / /cart 1625 / /account/orders 1322: only font-family, --vc-radius consumers (8->16, 6->14) and 7 footer titles (uppercase->none) differ; font-size, line-height (computed string), font-weight, letter-spacing, font-style, box-shadow 0 differences; runtime --color-* 0 differences light and dark (230 of 238 differ light->dark inside each build, so the comparison is mode-sensitive). Of 242 sampled root custom properties only --vc-radius, --vc-base-font-family, --vc-heading-font-family (plus the font-family alias) differ. Storybook h1-h6 (VcMarkdownRender headings story): soft changes family only (Lato->Geologica), size/line-height/weight/case/spacing equal classic."
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534; setting:design=soft (local build); browser=Edge (Chromium) 1280+375, light+dark, anonymous and signed-in; baseline=vc-frontend dev@4ce61d0c classic
    at: 2026-11-02T10:15:19.775Z
    by: session:9e59a9c2
    bodyHash: 61d925a4da52
    who: Lenajava1
    note: "Edge: 11 anonymous + 11 signed-in pages in P x 1280/375 x light/dark, element-paired by DOM path, soft vs classic-BASE: only font-family, --vc-radius consumers and 7 footer titles differ; --vc-color-* 0 differences in every cell. Source grep: 0 colour properties in client-app/designs/soft. Classic-PR (vcst) vs classic-BASE: 0 differences in every cell."
---
Read at vc-frontend PR 2600 head a4f29534 (applies to every storefront page, home included). client-app/designs/soft/_tokens.scss sets, on html[data-design="soft"], exactly three tokens: --vc-radius: 1rem, --vc-base-font-family: Inter, sans-serif and --vc-heading-font-family: Geologica, sans-serif. client-app/designs/soft/blocks/footer-links.scss (layer vc.design) removes the uppercase of .footer-links__title. tailwind.config.ts wraps the radius (except none/full), shadow (one token per shadow part), font-size + line-height, line-height, font-weight and letter-spacing scales as var(--vc-<property>-<key>, <previous literal>). The heading rule in _typography-roles.scss reads --vc-heading-font-weight/-font-style/-text-transform/-letter-spacing with fallback inherit. Soft sets none of these scale or role tokens, so under soft they all resolve to the classic values. Tailwind rounded-* utilities therefore stay classic, and only component SCSS that reads var(--vc-radius) directly turns 1rem. No soft value is scoped to dark mode.
