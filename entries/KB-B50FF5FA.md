---
id: KB-B50FF5FA
subject: The storefront sends GetMenu(name = the CatalogMenuLinkListName setting, e.g. catalog-menu) only when white-labeling mainMenuLinks in GetPageContext are empty; non-empty mainMenuLinks replace that call
plane: experiential
question: Why does one storefront send a GetMenu catalog-menu request on page load and another does not?
questions:
  - text: When does the storefront request the catalog menu link list?
  - text: "Where does the storefront header catalog menu come from: white labeling or a link list?"
  - text: Why is there one fewer GetMenu call on a stand with white-labeling main menu links?
status: active
appliesTo:
  - axis: buyer
    value: guest
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.menu
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: b8b4b94cd1e4180f
    conditions: theme=vc-theme-b2b-vue-2.63.0-pr-2630-a18a-a18a3bb2; setting:site_builder_enabled=false; setting:CatalogMenuLinkListName=catalog-menu; compared-with=vcst_dev theme 2.63.0-alpha.2663
    at: 2026-11-19T18:55:36.755Z
    by: session:51ac2fe3
    who: Lenajava1
---
Cold anonymous loads of / on two B2B-store storefronts, Chromium, cache disabled. Both read the store setting CatalogMenuLinkListName = catalog-menu. vcst_qa (theme 2.63.0-pr-2630, site_builder_enabled off): GetPageContext answers whiteLabelingSettings.mainMenuLinks with items (Alcoholic Drinks, ...); the page sends GetMenu only for pinned-links and footer-links, and the header catalog menu shows the white-labeling links. vcst_dev (theme 2.63.0-alpha.2663): mainMenuLinks = []; the page also sends GetMenu(name: catalog-menu). The source agrees on both builds: useNavigations.fetchCatalogMenu uses white-labeling mainMenuLinks first and calls getMenu(CatalogMenuLinkListName) only when they are empty (then falls back to child categories). So a per-page GraphQL census differs by one GetMenu between stands with and without white-labeling main menu links; that is data, not a build difference.
