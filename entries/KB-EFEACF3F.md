---
id: KB-EFEACF3F
subject: ConfigurationSectionInput takes customText directly; a textDetails wrapper is rejected as an undefined field
plane: experiential
question: How is custom text for a Text section passed to addItem configurationSections?
questions:
  - text: Is textDetails a field of ConfigurationSectionInput?
  - text: addItem configurable Text section customText input shape
status: active
appliesTo:
  - axis: feature
    value: configurable-products
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.addItem
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    at: 2026-10-28T07:16:11.948Z
    by: session:624d35a1
    who: kutasinaelena
---
On vcptcore_regression (2026-10-28), Mutation.addItem with configurationSections: [{sectionId, type: "Text", textDetails: {customText: "..."}}] failed GraphQL validation: 'Field "textDetails" is not defined by type "ConfigurationSectionInput"', so no line was added. The same call with configurationSections: [{sectionId, type: "Text", customText: "..."}] was accepted and the line carried the text (configurationItems[].customText non-null). Cases and fixtures must pass customText as a direct field of the section input.
