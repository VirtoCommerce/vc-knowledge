---
id: KB-7CAAD492
subject: With design soft (vc-frontend PR 2600) the /cart Discount & coupons widget turns 16px while its coupon cards stay 4px, and the /account/orders active nav item stays 6px inside a 16px widget, because Tailwind rounded-* utilities read the unset --vc-radius-* scale, not --vc-radius
plane: experiential
question: Under the soft design, do cart coupon cards and the account nav follow the widget's 16px radius?
questions:
  - text: Why are coupon cards 4px in a 16px widget with design soft?
  - text: Do Tailwind rounded utilities follow --vc-radius in the soft design?
  - text: Is the active account navigation item rounded like its widget under soft?
status: active
appliesTo:
  - axis: design
    value: soft
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534 (local build); setting:design=soft
    at: 2026-11-02T11:21:35.457Z
    by: session:be9f7d04
    who: Lenajava1
    note: "distinct: KB-9045E101 covers only the CyberSource fields; this entry is the coupon cards inside the Discount & coupons widget and the /account/orders active nav item, i.e. Tailwind rounded-* utilities not following --vc-radius."
---
Local production build of vc-frontend PR 2600 at a4f29534 with design soft, vcst backend, observed in Chromium and Firefox at 1920, light and dark, signed in. Soft sets --vc-radius: 1rem, so components that read var(--vc-radius) turn 16px (.vc-widget) or 14px (.vc-button). Tailwind rounded-* utilities read the --vc-radius-* scale tokens, which soft leaves unset, so they keep their classic values. On /cart the Discount & coupons .vc-widget is 16px while its 5 div.coupon-card are 4px (classic base: 4px cards in an 8px widget). On /account/orders the active navigation item is 6px inside a 16px widget.
