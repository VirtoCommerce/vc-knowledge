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
  - method: observation
    deployment: vcptcore_qa1
    conditions: module:VirtoCommerce.XCart=3.1043.0-alpha.300-vcst-5888-punchoutorderrequest
    at: 2026-10-28T22:29:01.868Z
    by: session:54733023
    bodyHash: 97471eabbb27
    who: kutasinaelena
    note: "2026-10-29, AGENT-TEST-CFG-017 (one required Text section): addItem with configurationSections [{sectionId, type:\"Text\", customText}] added line SKU Configuration-AGENT-TEST-CFG-017 with configurationItems[0].customText set; the same addItem without configurationSections returned no GraphQL error and added no line."
---
On vcptcore_regression (2026-10-28), Mutation.addItem with configurationSections: [{sectionId, type: "Text", textDetails: {customText: "..."}}] failed GraphQL validation: 'Field "textDetails" is not defined by type "ConfigurationSectionInput"', so no line was added. The same call with configurationSections: [{sectionId, type: "Text", customText: "..."}] was accepted and the line carried the text (configurationItems[].customText non-null). Cases and fixtures must pass customText as a direct field of the section input.
