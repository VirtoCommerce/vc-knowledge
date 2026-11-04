---
id: KB-4C17EE4B
subject: On vcptcore_regression (theme 2.59.0) no product page renders the VcVariantPicker; variation masters render the B2B variations list instead
plane: experiential
question: Which product page on vcptcore_regression renders the VcVariantPicker variant picker, and what does a variation master's PDP render instead?
questions:
  - text: Is there a VcVariantPicker fixture product on vcptcore_regression?
  - text: Why is A11Y-VCP-001 blocked on vcptcore_regression?
  - text: What does a variation product PDP render on B2B-store — variant picker or variations table?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.product
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; theme=2.59.0; store=B2B-store
    at: 2026-11-04T21:41:26.616Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
---
During the stable-16 W6 regression (case A11Y-VCP-001) runners looked for a product detail page that renders the VcVariantPicker (variant picker built from variation properties). None was found on this stand: variation masters such as the bolts product with 3 variations render the B2B "Product variations" list/table, a seeded configurable test product renders an empty configurable radiogroup (empty Variation options is by design, KB-35375A55), and the configurable caps/shirts product used as the picker fixture on other stands is not present (404). The theme source does ship the picker (ui-kit molecules/variant-picker/vc-variant-picker.vue, used by shared/catalog/components/product/options.vue — read on the default branch, not verified at tag 2.59.0). Consequence: a picker test on this stand needs a seeded product whose variations carry the properties that drive the picker; without it the case is BLOCKED on missing data, not a product failure.
