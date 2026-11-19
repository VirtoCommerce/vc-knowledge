---
id: KB-DD7733EE
subject: Site Builder designer's "The page couldn't be saved" toast stays about 5 minutes and survives navigation and a later successful save
plane: experiential
question: How long does the Site Builder designer save-error toast stay visible?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/site-builder-shell
  - coordinate: POST /api/site-builder/designer/save
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0
    defect: suspected
    at: 2026-11-19T14:31:27.687Z
    by: session:51ac2fe3
    who: Lenajava1
---
After a refused save in the Site Builder Vue designer (for example an invalid palette colour, POST /api/site-builder/designer/save → 400), the toast "The page couldn't be saved" stays on screen for about 5 minutes, survives switching page or template, and is still shown after a later save returns 200. Observed in two VCST-6571 lanes.
