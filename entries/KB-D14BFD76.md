---
id: KB-D14BFD76
subject: The header search results dropdown stays open while Tab moves through its product links, closes on Tab past its last link or a click outside, but stays open when Shift+Tab moves focus from the search input back to the logo
plane: experiential
question: When does the storefront header search dropdown close as keyboard focus or a click leaves it?
questions:
  - text: Does Tab out of the header search dropdown close it?
  - text: Does Shift+Tab from the header search box close the results dropdown?
  - text: Does clicking outside close the header search dropdown?
status: active
appliesTo:
  - axis: element
    value: header-search
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcst_qa
    build: 8d4bda9222c60036
    conditions: theme=2.61.0-pr-2598-608d-608d6e2f; role=anonymous guest; browser=chromium
    at: 2026-10-26T10:40:20.341Z
    by: session:1c2d4240
    who: Lenajava1
---
Guest, Chromium 1920, phrase 'epson' (8 products). Theme 2.61.0-pr-2598-608d (local stack proxied to vcst_qa): Tab from the input goes Clear -> Search -> each product title link (image links are not tab stops); the dropdown stays open throughout. Tab past the 8th link moved focus to 'Bulk order' and the dropdown closed (seen closed within ~1 s). A click on the footer closed it. Shift+Tab from the search input to the header logo link left the dropdown open (still open after 2 s). The same Shift+Tab result was seen on the vcst_qa storefront's own deployed theme, so it is not introduced by PR 2598.
