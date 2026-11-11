---
id: KB-70FEDC01
subject: On /cart the Pick points dialog's locations radiogroup has the raw i18n key 'pages.checkout.shipping.links.select_pickup_point' as its accessible name
plane: experiential
question: What accessible name does the pickup-locations radiogroup in the /cart Pick points dialog have?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4605db2ed710c357
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2642; store=B2B-store; role=signed-in B2B buyer; browser=chromium
    defect: suspected
    at: 2026-11-11T16:01:56.395Z
    by: session:7e403256
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: 4605db2ed710c357
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2642; store=B2B-store; role=signed-in B2B buyer; browser=chromium
    at: 2026-11-11T16:18:22.674Z
    by: session:7e403256
    bodyHash: e1f2a0fac490
    defect: suspected
    who: kutasinaelena
    note: "Re-verified independently. Raw key appears only as the aria-label, nowhere in visible text. Source: vc-frontend select-address-map-list.vue:6 uses $t('pages.checkout.shipping.links.select_pickup_point'), a path absent from locales/en.json; the existing string is shared.checkout.shipping_details_section.links.select_pickup_point. Introduced by #2230 (VCST-4703), present in release 2.61.0."
---
Signed-in B2B buyer on /cart with one line, Delivery option Pickup, clicked 'select a pickup address'. The 'Pick points' dialog opens with Country/State/City filters, a map and a radiogroup of locations (Load more paging). In the accessibility tree the radiogroup is named 'pages.checkout.shipping.links.select_pickup_point' - an untranslated i18n key, not a human label. Selecting a location and 'Pick up here' works (shipping cost 0.00). Seen on two consecutive builds (theme 2.62.0-alpha.2642 here, also a prior run). Screen-reader users hear the key.
