---
id: KB-BB5C5B6D
subject: "Site Builder: a product template made with \"Create template (copy of default)\" holds product-info with an empty blocks list, so a PDP routed to it loses the Properties and Product variations blocks"
plane: experiential
question: Does a Site Builder product template created as a copy of default render the same product page as the default template?
questions:
  - text: Why does a product with variations show no variation list on a rule template?
  - text: What content does Create template (copy of default) write for product templates?
status: active
appliesTo:
  - axis: surface
    value: admin-designer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/site-builder/designer/save
  - coordinate: Query.themeSnapshot
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; store=B2B-store
    defect: suspected
    at: 2026-11-19T13:59:24.188Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; platform=3.1099.0; store=B2B-store
    at: 2026-11-19T14:50:06.393Z
    by: session:51ac2fe3
    bodyHash: ad5ef2ef13ce
    defect: suspected
    who: Lenajava1
    note: "Re-reproduced with a fresh template (product.agent-test-6571-inv-b2): the designer/save request body already carries product-info with blocks [] and no related/recommended/reviews sections, so the server stores what the client sends. Rule inCategory Carriage Bolts, publication 15: product with 3 variations shows no Properties, no variations list, no quantity steppers; rule removed, publication 16: same PDP on Default shows Properties and 3 quantity steppers."
---
B2B-store: created product template Y via the designer's 'Create template (copy of default)'. The saved file default/templates/product.<key>.json contains product-info with variationsLayout b2b and "blocks": []. A rule sent the carriage-bolt category to Y and the theme was published: the PDP of a product with variations (two buyable variations) showed only the image, 'Variations in cart: 0' and the sidebar actions; no Properties block, no Product variations list, no quantity steppers. The same product on the Default template shows Properties and the Product variations list with quantity steppers. A simple product on Y still showed its quantity stepper and could be added to the cart. Default product template file default/templates/product.json is absent (GET template ... draft=true answers 404); the built-in default comes from the storefront.
