---
id: KB-D56DECE5
subject: At 375px the expanded white-labeling footer accordion reveals child links only 18 CSS px tall (280x18, 8px apart)
plane: experiential
question: What tap-target size do the storefront footer child links have on mobile after expanding a footer section?
questions:
  - text: Are mobile footer links at least 44x44 px on the storefront?
  - text: How big are the footer accordion headers and links at 375px?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: mobile
anchors:
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: theme=2.63.0; platform=3.1101.0; store=B2B-store; browser=chromium
    at: 2026-11-27T13:36:34.061Z
    by: session:d2ee4088
    who: Lenajava1
---
On vcst_qa (theme 2.63.0), 375x667 Chromium (clientWidth 360), signed in as an organization member with a two-level white-labeling footer list: the section headers render as accordion buttons 280x40; tapping one reveals its child links as block anchors 280x18 CSS px with an 8px gap (centres 26px apart). They meet the WCAG 2.2 2.5.8 24px spacing exception but are below a 44x44 target (2.5.5).
