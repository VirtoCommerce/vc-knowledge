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
  - method: observation
    deployment: vcptcore_regression
    build: 5609fc0c36e7302c
    conditions: platform=3.1076.7; module:VirtoCommerce.XCart=3.1039.0; role=signed-in buyer
    at: 2026-10-21T13:07:43.519Z
    by: session:624d35a1
    bodyHash: 652bfa1fc8d3
    who: kutasinaelena
    note: "Re-observed 2026-10-21 on 3 fresh carts. The singular updateConfigurationItem and the plural updateConfigurationItems both append (245 -> 325, two items, itemsCount 1, no errors). The Product-section control replaces (365 -> 575). Source: CartAggregate.FindConfigurationItem keys Variation items by Type+SectionId+ProductId (\"multiple variations can exist\", PR #101), so this is intended, not a defect. Swapping works as removeConfigurationItem(old) then updateConfigurationItem(new), which gives one item and price 280."
---
2026-10-15 on vcptcore_regression, signed-in B2B buyer, xAPI. addItem of a configurable parent (base 200 USD) with configurationSections [{sectionId, type:"Variation", option:{productId: variation Red (sale 45)}}] gave one configurationItem and price 245. updateConfigurationItem on the same lineItemId and same sectionId with option Blue (80) returned NO errors but configurationItems held BOTH Red and Blue and price 325 (200+45+80); itemsCount unchanged. Same sequence on a Product-type section (Seat 15 -> Engine 225 on a 350 base) REPLACED the item: one configurationItem (Engine), price 575. So for a Variation section the update behaves as an add; the case expectation 'update replaces the variation' fails there. Variation-type sections are not described in the VirtoOZ product-configuration user guide (only Product/Text/File).
