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
---
Read at vc-frontend PR 2600 head a4f29534 (applies to every storefront page, home included). client-app/designs/soft/_tokens.scss sets, on html[data-design="soft"], exactly three tokens: --vc-radius: 1rem, --vc-base-font-family: Inter, sans-serif and --vc-heading-font-family: Geologica, sans-serif. client-app/designs/soft/blocks/footer-links.scss (layer vc.design) removes the uppercase of .footer-links__title. tailwind.config.ts wraps the radius (except none/full), shadow (one token per shadow part), font-size + line-height, line-height, font-weight and letter-spacing scales as var(--vc-<property>-<key>, <previous literal>). The heading rule in _typography-roles.scss reads --vc-heading-font-weight/-font-style/-text-transform/-letter-spacing with fallback inherit. Soft sets none of these scale or role tokens, so under soft they all resolve to the classic values. Tailwind rounded-* utilities therefore stay classic, and only component SCSS that reads var(--vc-radius) directly turns 1rem. No soft value is scoped to dark mode.
