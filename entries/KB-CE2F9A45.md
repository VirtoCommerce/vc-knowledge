---
id: KB-CE2F9A45
subject: "Missions SKU modal at 375px: footer is visually Add to cart above Close but the DOM/tab order is Close then Add to cart; stepper buttons sit 1 px from the quantity input"
plane: experiential
question: What are the tab order and target gaps in the missions SKU modal on mobile?
questions:
  - text: On a phone, does tabbing through the mission product popup go in the order the buttons appear?
  - text: /account/missions SKU modal 375 tab order Close Add to cart DOM order
  - text: How large is the gap between the quantity stepper buttons and the input in the missions SKU modal?
  - text: How do cart subtotal and Total units render when zero in the missions SKU modal?
concepts:
  - id: accessibility
  - id: mobile-layout
  - id: loyalty-mission
status: active
appliesTo:
  - axis: viewport
    value: 375
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/missions
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-01T16:51:52.455Z
    by: session:e04c1f9b
    who: Lenajava1
---
Build 2.59.0-pr-2524, Edge, 375x812, /account/missions, SKU mission modal opened through the card Open mission button. Footer buttons: Add to cart (top 700, 327x44) is rendered above Close (top 752, 327x44) but Close precedes Add to cart in DOM order, so Tab goes bottom-to-top relative to the visual order (WCAG 2.4.3). The quantity stepper Decrease (32x32) / input (56x30) / Increase (32x32) have 1 px gaps (BL-UI-006 asks 8 px; sizes pass the 24 px WCAG 2.5.8 minimum). Cart subtotal and Total units render a bare 0 while the row total renders $0.00.
