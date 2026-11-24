---
id: KB-9FDDB07C
subject: "Site Builder product template rules: the flag-on storefront PDP uses the first enabled rule that matches; reordering or disabling a rule takes effect on the next theme publish"
plane: experiential
question: Which template does the storefront product page use when several Site Builder template rules match the same product?
questions:
  - text: Does the order of Site Builder template rules matter?
  - text: What happens on the PDP when the first matching template rule is disabled?
  - text: How does themeSnapshot rules.product decide the product template?
status: active
appliesTo:
  - axis: surface
    value: admin-designer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.themeSnapshot
  - coordinate: POST /api/site-builder/theme/publish
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; store=B2B-store
    at: 2026-11-19T13:59:24.116Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 54b87331b2bf6e07
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-644f; theme=2.63.0-pr-2676-f03c-f03c41eb; store=B2B-store
    at: 2026-11-24T14:22:18.786Z
    by: session:bd155cea
    bodyHash: cf5dce4a4516
    who: Lenajava1
    note: "Re-seen on SiteBuilder 644f + theme f03c: adding Products in Carriage Bolts -> Fasteners at the top made the designer show 'Site uses: Fasteners (rule 1)'; after Publish theme the carriage-bolt PDP used Fasteners; deleting the rule and publishing again made it 'Default (no rule matches)'."
---
B2B-store, Vue designer Template rules panel: two rules both 'Products of type Digital', the first to template Y, the second to template Z (each template carries a distinct Text section title). After Publish theme, the PDP of a simple Digital product rendered Y. Dragged Z above Y and published: PDP rendered Z. Switched the Z rule off (Rule on toggle) and published: Query.themeSnapshot rules.product carried the Z rule with "disabled": true and the PDP rendered Y again. The rule list is the theme file config/template_rules.json; with no list of its own the theme uses default rules (one rule default-b2c on property VirtoFrontend_UI_Layout=B2C). The product rule form offered three condition kinds only: In categories, Product type, Property (the designer bundle also knows path, language, source, setting, but shows conditions per page type as announced by the storefront preview).
