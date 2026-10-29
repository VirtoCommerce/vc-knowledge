---
id: KB-E3CF4CB5
subject: The Page Builder shell shows an empty full-viewport screen at viewport width 1024 px or less; at 1025 px it renders normally
plane: experiential
question: Does the Page Builder shell work at a 1024 px wide viewport?
questions:
  - text: Why is the page builder shell blank on a tablet-width window?
  - text: At which width does the page builder shell switch to its mobile layout?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: viewport
    value: 1024
anchors:
  - coordinate: /apps/page-builder-shell
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: ed1246f57f06adbd
    conditions: module:VirtoCommerce.PageBuilderModule=3.1041.0-pr-179-5e6c; browser=chromium
    defect: suspected
    at: 2026-10-29T13:48:54.858Z
    by: session:9b657bc7
    who: Aleksandra-Mitricheva
---
Signed in to /apps/page-builder-shell/?storeId=<store> as admin in Chromium. At 1920, 1280 and 1025 px wide the list and details blades render. At 1024x768 (after a resize and after a full reload) the viewport is an empty light-grey screen with only the environment badge; the accessibility tree still holds the blades, but an empty .menu-sidebar__wrapper (z-index 10) covering 0,0-1024x768 is what elementFromPoint returns, and no menu or close control is exposed. The same blank state was seen in Firefox in an earlier run on the same stand, so it is not browser-specific.
