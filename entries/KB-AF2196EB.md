---
id: KB-AF2196EB
subject: In VcStatWidget a long unbreakable token in errorText, description or delta stays on one line and paints past the card border, while the same token in label wraps inside the card
plane: experiential
question: Does a long unbreakable word in VcStatWidget errorText, description or delta wrap inside the card or overflow past its border?
questions:
  - text: Does a URL in VcStatWidget errorText fit inside the card?
  - text: Which VcStatWidget text parts wrap long words?
  - text: Does the VcStatWidget error message overflow the card in Storybook?
status: active
appliesTo:
  - axis: component
    value: vcstatwidget
  - axis: host
    value: storybook
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /story/components-organisms-vcstatwidget--error-state
  - coordinate: /story/components-organisms-vcstatwidget--basic
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4661c4a61799a028
    conditions: storybook=2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676; commit=vc-frontend@bfe0b259; browser=edge-chromium
    defect: suspected
    at: 2026-10-20T12:20:59.263Z
    by: session:43a7a721
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 4661c4a61799a028
    conditions: storybook=2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676; commit=vc-frontend@bfe0b259; browser=firefox
    at: 2026-10-20T12:34:57.859Z
    by: session:43a7a721
    bodyHash: e6f5bf4ad5d5
    defect: VCST-6448
    who: Lenajava1
    note: "First seen on firefox in the exploratory session (41-char word: errorText ~77px, delta ~73px, description ~8px past the border at 768 and 375); filed as VCST-6448."
---
QA Storybook, Edge (Chromium), stories components-organisms-vcstatwidget--error-state and --basic (card 256px wide, wrapper max-w-64), viewports 1280 and 375. I typed a 41-character token with no spaces into the Controls for errorText, description and delta. Measured with a text Range against the card's border-box right edge: errorText +82px, delta +78px, description +11px, each on one line, identical at both viewports. The card has overflow visible, so the text paints outside the border and is not clipped. The same token typed into label wraps to 2 lines inside the card in the same render. Computed styles: label has overflow-wrap break-word and min-width 0. The errorText and delta spans are flex items with min-width auto and overflow-wrap normal, and description is a block with overflow-wrap normal. Realistic input: a 'Request failed: ' message ending in a 50-character URL overflows in errorText by +41px. A message with an order number and a 26-character German compound stay inside. This is prose wrapping. The figure (value) staying on one line is a separate, by-design decision, see KB-069099FD.
