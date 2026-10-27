---
id: KB-F2DBAF5C
subject: "On theme 2.61.0-pr-2577 (CSS cascade layers), a keyboard-focused KPI card in /company/dashboard Edit layout shows no focus ring: the SalesRep plugin's unlayered dashed edit outline beats the theme's layered global focus ring"
plane: experiential
question: Does a Tab-focused stat card on /company/dashboard in Edit layout show a visible focus ring on the cascade-layers theme build?
questions:
  - text: Why is there no focus outline on Sales Rep hub layout blocks in edit mode?
  - text: Is the Sales Rep hub CSS served from the theme or from the VirtoCommerce.SalesRep module plugin?
  - text: Do federated module plugin styles override the layered theme CSS?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-layout
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    conditions: theme=2.61.0-pr-2577-4c17-4c171a96; store=B2B-store; browser=chromium; salesrep-frontend=module-plugin
    defect: suspected
    at: 2026-10-27T22:03:04.319Z
    by: session:3e14e295
    who: Lenajava1
---
Signed in as a sales rep on /company/dashboard, Edit layout, 1920px. Tab to the first KPI card (div.layout-block.layout-block--editing.layout-block--whole, tabindex 0, aria-label 'Reorder Orders in “New” status', :focus-visible true): computed outline is the edit-mode 'dashed 1px neutral-300' (same as unfocused cards), not the focus ring (2px solid #3b82f6). The hub's CSS is loaded from /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/assets/*.css (module-federation plugin), unlayered, and holds no .layout-block--whole:focus-visible rule; the theme's global ring is 'body :focus-visible' inside @layer base.tw, and an unlayered rule outranks any layered one regardless of specificity. Before the layers change the theme rule was unlayered (0,1,1) and beat .layout-block--editing (0,1,0). The theme repo's own copy of the sales-rep module adds the focus-visible rule, but the stand renders the plugin copy. Reorder handle buttons and other controls still show the 2px blue ring; view mode has no focusable blocks.
