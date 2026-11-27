---
id: KB-ABAEAFCD
subject: On desktop the storefront header menubar renders white-labeling mainMenuLinks as top-level links only; an entry with childItems opens no dropdown on hover, click or Enter and navigates to its own url
plane: experiential
question: Does an organization main-menu entry with childItems open a dropdown in the desktop storefront header?
questions:
  - text: Where are white-labeling main menu child items shown on desktop?
  - text: Does hovering a main menu entry with children show a submenu in vc-frontend 2.63?
  - text: What happens when you click a main-menu parent entry in the storefront header?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: desktop
anchors:
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; theme=vc-theme-b2b-vue-2.63.0; store=B2B-store; browser=chromium
    at: 2026-11-27T12:50:15.626Z
    by: session:d2ee4088
    who: Lenajava1
---
vcst_qa, theme 2.63.0, Chromium 1920x1080, signed in as a member of an organization whose enabled white-labeling record has a main-menu list (Home, Brands with children Laptops/Phones/Tablets, Company, Support). The header 'Main navigation' > menubar 'Navigation' shows menuitems Support, Company, Brands, Home. Hovering Brands (2s) renders no child item in the DOM; clicking Brands or pressing Enter on it navigates to /brands. A guest sees the store menu (catalog-menu, 15 entries) and hovering 'Alcoholic Drinks' (which has 6 children) also opens nothing. Child items of mainMenuLinks are therefore not reachable from the desktop header; on mobile they are reachable through the Main menu drawer (Catalog > entry > children). The vc-frontend 2.63.0 source (shared/layout/components/header/_internal/mega-menu.vue) renders only item.title links in that menubar.
