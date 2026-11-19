---
id: KB-1D8E4BFA
subject: On a multilanguage page version, the Site Builder designer's Add section lists "No sections match" even though the storefront preview has loaded
plane: experiential
question: Can sections be added to a multilanguage page in the Site Builder Vue designer?
questions:
  - text: Why does Add section show No sections match on a Site Builder page?
status: active
appliesTo:
  - axis: surface
    value: admin-designer
anchors:
  - coordinate: /apps/site-builder-shell
  - coordinate: GET /api/site-builder/catalog
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; store=B2B-store; page=multilanguage
    defect: suspected
    at: 2026-11-19T13:59:24.532Z
    by: session:51ac2fe3
    who: Lenajava1
---
B2B-store: created a multilanguage page (en-US and de-DE versions) and opened en-US with 'Edit in designer'. The preview iframe loaded the storefront (header, footer, breadcrumbs) and the catalog request GET /api/site-builder/catalog answered with 25 section schemas, yet Add section (and a reload) showed 'No sections match' with an empty search. On product templates of the same store, Add section listed sections (Text, Related products, Announcement bar in the header group). Page settings and Layout worked.
