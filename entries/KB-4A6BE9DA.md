---
id: KB-4A6BE9DA
subject: The Admin Shell subscription blade shows a read-only Configuration card for a configured subscription (option per section, configured price, captured-from order link, fingerprint); a simple subscription shows no card
plane: experiential
question: How does the Admin Shell show the configuration of a configurable-product subscription?
questions:
  - text: Can an admin edit the configuration of a product subscription?
  - text: What does the Configuration card on a product subscription blade contain?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/product-subscriptions/product-subscription/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 1dfb1b0bb5149a92
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-cb70; vc-shell=2.10.4
    at: 2026-11-19T15:10:40.975Z
    by: session:d9b8e380
    who: kutasinaelena
---
Admin Shell, More > Subscriptions, open a subscription created from a configured line: between Schedule and Customer and delivery a "Configuration" card lists the section name (Handlebars) -> "<option product name> x 1 · <option sku>", "Configured price" $266.00, "Captured from" a link to the first order, "Fingerprint" (abbreviated configurationHash, e.g. 1701…c69). The card is a definition list with no inputs; Save stays disabled. A subscription of a simple product created before the configuration migration shows no Configuration card (Schedule then Customer and delivery) and no console error.
