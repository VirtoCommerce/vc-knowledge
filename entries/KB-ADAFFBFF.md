---
id: KB-ADAFFBFF
subject: removeConfigurationItem needs the option productId to remove a Product section
plane: experiential
question: Does removeConfigurationItem remove an optional Product section when called with only sectionId and type?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.removeConfigurationItem
  - coordinate: Mutation.removeConfigurationItems
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:15:42.574Z
    by: session:memimpor
    who: Lenajava1
---
removeConfigurationItem and removeConfigurationItems return 200 with an empty errors array but leave the configuration unchanged when a Product-type section is sent as just sectionId plus type: validation requires option.productId, the failure goes to cart validationErrors as CONFIGURATION_SECTION_PRODUCT_REQUIRED, and the mutation returns early. Sent with the item's option.productId, an optional Product section is removed. A required section cannot be removed either way (CONFIGURATION_SECTION_REQUIRED), which is correct. The narrow silent-failure case was closed as won't fix, since a working call shape exists.
