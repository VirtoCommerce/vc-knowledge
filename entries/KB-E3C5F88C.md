---
id: KB-E3C5F88C
subject: Storefront /compare 'Clear all' is disabled while the compare list is empty, so an unconditional 'click Clear all' step cannot run on a clean profile
plane: experiential
question: Can the Clear all button on /compare be clicked when the compare list is already empty?
status: active
appliesTo:
  - axis: feature
    value: product-compare
  - axis: surface
    value: storefront-ui
  - axis: user
    value: guest
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    conditions: theme=2.63.0-pr-2666-7e02-7e022f10
    at: 2026-11-18T13:11:04.255Z
    by: session:25a30bd6
    who: Lenajava1
---
2026-11-18, guest, fresh browser context: /compare with 0 of 5 added shows the empty state with the 'Clear all' button rendered disabled (class vc-button--disabled), and Playwright click times out on it. With 2 products added it is enabled and opens a confirmation dialog with an 'OK' button; confirming empties the list.
