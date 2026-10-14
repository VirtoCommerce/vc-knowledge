---
id: KB-DFB2A4B2
subject: updateConfigurationItem on a Variation-type section appends the new variation next to the old one (price adds both), while on a Product-type section it replaces the selection
plane: experiential
question: Does Mutation.updateConfigurationItem replace or add the selected option of a configurable cart line, and does it differ by section type?
status: active
appliesTo:
  - axis: feature
    value: configurable-products
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.updateConfigurationItem
  - coordinate: Mutation.addItem
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T23:33:54.604Z
    by: session:624d35a1
    who: kutasinaelena
---
2026-10-15 on vcptcore_regression, signed-in B2B buyer, xAPI. addItem of a configurable parent (base 200 USD) with configurationSections [{sectionId, type:"Variation", option:{productId: variation Red (sale 45)}}] gave one configurationItem and price 245. updateConfigurationItem on the same lineItemId and same sectionId with option Blue (80) returned NO errors but configurationItems held BOTH Red and Blue and price 325 (200+45+80); itemsCount unchanged. Same sequence on a Product-type section (Seat 15 -> Engine 225 on a 350 base) REPLACED the item: one configurationItem (Engine), price 575. So for a Variation section the update behaves as an add; the case expectation 'update replaces the variation' fails there. Variation-type sections are not described in the VirtoOZ product-configuration user guide (only Product/Text/File).
