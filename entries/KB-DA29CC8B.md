---
id: KB-DA29CC8B
subject: Store-level white-labeling on a Site Builder store overrides the published theme palette, logo, category menu and footer menus for every visitor; the designer marks those fields
plane: experiential
question: Is a published Site Builder theme palette or logo visible on a store that has store-level white-labeling?
questions:
  - text: Why does the storefront not show the palette published from the Site Builder Theme tab?
  - text: Which Theme tab fields show 'Set by white-labeling for this visitor'?
status: active
appliesTo:
  - axis: surface
    value: admin-designer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/white-labeling/store/{id}
  - coordinate: Query.themeSnapshot
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; store=B2B-store; setting:WhiteLabeling.WhiteLabelingEnabled=true
    at: 2026-11-19T13:59:35.473Z
    by: session:51ac2fe3
    who: Lenajava1
    note: "distinct: KB-A23A4A41 is about organization vs store white-labeling merging in xAPI; this is about store white-labeling vs the Site Builder theme snapshot on the flag-on storefront and the designer's override marking."
---
B2B-store has an enabled store white-labeling record (GET /api/white-labeling/store/B2B-store: logo set, themePresetName Mercury, footer-links and catalog-menu lists). After publishing a generated Site Builder palette (primary #1e6b52, then #7a1fa2), a buyer of an organization without white-labeling saw --color-primary-500 #f99e24 in the :root:not(.dark) and html.dark rules of the vc-theme-variables stylesheet and the store white-labeling logo; the theme palette never reached the page. The designer marks Logo, Logo on dark backgrounds, Colors, the header Category menu and footer Menu blocks with "Set by white-labeling for this visitor: a change here doesn't show."
