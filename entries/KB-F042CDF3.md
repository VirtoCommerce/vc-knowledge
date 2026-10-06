---
id: KB-F042CDF3
subject: "On the storefront select-return-items page, a non-returnable line's reason text uses text-warning-700 (#ab660e): 4.53:1 on a white row but 4.34:1 on the zebra-striped row (#fafafa), so it fails WCAG 1.4.3 on even rows"
plane: experiential
question: Does the non-returnable line reason text on /account/returns/new/{orderId} meet 4.5:1 contrast?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: desktop
anchors:
  - coordinate: /account/returns/new/{orderId}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-06T17:04:06.055Z
    by: session:99b7bb2c
    who: kutasinaelena
---
On theme 2.59.0-pr-2532 at 1920px, starting a return from an order with a cancelled line shows that line with a dash in place of the quantity input and the reason 'This line was cancelled' in text-warning-700 (#ab660e). When the line sits in a striped table row (background #fafafa), the contrast is 4.34:1 and axe-core color-contrast reports it (4.33). At 375px the line renders as a card on white, where it measures 4.53:1 and axe passes. The neutral secondary texts on the same page (eligible count, SKU, ordered/returnable values) are neutral-600 #525252, which is 7.49 to 7.81:1.
