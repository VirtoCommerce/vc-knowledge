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
  - method: observation
    deployment: vcst_qa
    at: 2026-10-06T16:36:03.509Z
    by: session:7d6bccc9
    who: Lenajava1
    note: "2026-10-06: POST with `sections` + isActive:true saved an 8-section mixed Product/Text/File configuration; read back isActive=true, 8 sections. Text-section predefined presets persist via options[].text with productId null."
---
The create/update body takes the sections array in a field named sections and needs isActive true. A body using another name such as configurationSections is accepted with 200, but the unknown field is dropped and the configuration is saved with no sections and auto-deactivated; search then finds it with sections empty and isActive false, and xAPI shows no configuration sections. Product-type sections round-trip correctly, with exactly one option normalised to isDefault. Sending two isDefault options over REST returns 500. A Variation-type section was saved with its name and type but its options came back empty on every read, with no validation error even for a non-existent option product.
