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
---
Read at vc-frontend PR 2600 head a4f29534 (applies to every storefront page, home included). client-app/designs/soft/_tokens.scss sets, on html[data-design="soft"], exactly three tokens: --vc-radius: 1rem, --vc-base-font-family: Inter, sans-serif and --vc-heading-font-family: Geologica, sans-serif. client-app/designs/soft/blocks/footer-links.scss (layer vc.design) removes the uppercase of .footer-links__title. tailwind.config.ts wraps the radius (except none/full), shadow (one token per shadow part), font-size + line-height, line-height, font-weight and letter-spacing scales as var(--vc-<property>-<key>, <previous literal>). The heading rule in _typography-roles.scss reads --vc-heading-font-weight/-font-style/-text-transform/-letter-spacing with fallback inherit. Soft sets none of these scale or role tokens, so under soft they all resolve to the classic values. Tailwind rounded-* utilities therefore stay classic, and only component SCSS that reads var(--vc-radius) directly turns 1rem. No soft value is scoped to dark mode.
