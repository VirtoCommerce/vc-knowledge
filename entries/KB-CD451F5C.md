---
id: KB-CD451F5C
subject: the product configuration POST silently saves an empty inactive configuration for a wrong sections field
plane: experiential
question: What body does POST /api/catalog/products/configurations need, and what happens if the sections field is named wrong?
questions:
  - text: I set up a configurable product through the API but the shop shows no options, what went wrong?
  - text: Which field name must hold the sections array when creating a product configuration over REST?
  - text: Why is a product configuration saved inactive and empty after a 200 response?
  - text: What happens when a configuration section has two default options or is of Variation type?
  - text: Does the configuration endpoint validate option products or reject unknown fields?
concepts:
  - id: configurable-product
  - id: rest-api
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /api/catalog/products/configurations
  - coordinate: ProductConfiguration.sections
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:17:24.884Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-06T16:36:03.509Z
    by: session:7d6bccc9
    who: Lenajava1
    note: "2026-10-06: POST with `sections` + isActive:true saved an 8-section mixed Product/Text/File configuration; read back isActive=true, 8 sections. Text-section predefined presets persist via options[].text with productId null."
    build: c97ce3e35257b18e
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: module:VirtoCommerce.Catalog=3.1052.0; module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9
    at: 2026-11-18T13:08:41.272Z
    by: session:25a30bd6
    bodyHash: 4e7d54e7e27b
    who: Lenajava1
    contradicts: true
    contested: virtocommerce.catalog 3.1052.0 vs 3.1045.0; virtocommerce.xcatalog 3.1027.0-pr-107-5ca9 vs 3.1021.0
    note: "2026-11-18: the claim \"Sending two isDefault options over REST returns 500\" does not hold for a well-formed body. POST /api/catalog/products/configurations with id + productId (the configured product) + a section carrying id, configurationId, allowPredefinedOptions true and options that carry their own id, sectionId and productId (the option PRODUCT id), with two options isDefault true, returned 200 and a read-back had exactly one isDefault option and all three options kept. The 500 came from the payload (configurableProductId, an option id in options[].productId, section without allowPredefinedOptions). Without allowPredefinedOptions true the options are dropped silently (200), after which sending the old option ids answers 409."
---
The create/update body takes the sections array in a field named sections and needs isActive true. A body using another name such as configurationSections is accepted with 200, but the unknown field is dropped and the configuration is saved with no sections and auto-deactivated; search then finds it with sections empty and isActive false, and xAPI shows no configuration sections. Product-type sections round-trip correctly, with exactly one option normalised to isDefault. Sending two isDefault options over REST returns 500. A Variation-type section was saved with its name and type but its options came back empty on every read, with no validation error even for a non-existent option product.
