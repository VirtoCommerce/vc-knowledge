---
id: KB-CD451F5C
subject: the product configuration POST silently saves an empty inactive configuration for a wrong sections field
plane: experiential
question: What body does POST /api/catalog/products/configurations need, and what happens if the sections field is named wrong?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/catalog/products/configurations
  - coordinate: ProductConfiguration.sections
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:17:24.884Z
    by: session:memimpor
    who: Lenajava1
---
The create/update body takes the sections array in a field named sections and needs isActive true. A body using another name such as configurationSections is accepted with 200, but the unknown field is dropped and the configuration is saved with no sections and auto-deactivated; search then finds it with sections empty and isActive false, and xAPI shows no configuration sections. Product-type sections round-trip correctly, with exactly one option normalised to isDefault. Sending two isDefault options over REST returns 500. A Variation-type section was saved with its name and type but its options came back empty on every read, with no validation error even for a non-existent option product.
