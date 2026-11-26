---
id: KB-BA24D21F
subject: vc-frontend ui-kit build at fc383c4e3 compiles a fallback copy of the design palette into @layer theme (:root and html.dark)
plane: experiential
question: Does the vc-frontend ui-kit built CSS carry a fallback palette in @layer theme, and how does it differ from dev?
questions:
  - text: Which CSS layer holds the ui-kit fallback colour tokens?
  - text: What changed in compiled CSS when ui-kit tokens moved out of the app layer?
  - text: Is there a :root fallback palette in the vc-frontend build?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: source
    deployment: vc-frontend@fc383c4e3
    at: 2026-11-18T20:51:29.112Z
    by: session:feee22e7
---
Diffing the built storefront CSS of vc-frontend dev (e97860e64) against head fc383c4e3 (the ui-kit Step 2 change that moves component tokens, dark and print styles into ui-kit) shows that the head build adds a fallback copy of the colour palette inside @layer theme: 93 declarations on :root and 90 on html.dark, none of which exist on dev. Other differences beyond layer renames (vc.app→vc.kit, vc.design→vc.design.kit / vc.design.app): breadcrumbs --link-color now falls back to accent-600; the line-items subtotal colour now reads --vc-line-items-subtotal-color; #popover-host right:0 became inset-inline-end:0; the z-[9999] utility class is no longer emitted; popover v-bind hash class names changed. These are compiled-CSS facts only; whether the store preset still overrides the fallback at runtime has to be checked on a stand.
