---
id: KB-A4C7F35D
subject: "VcStatWidget treats a whitespace-only errorText as an error: it hides the figures and shows the alert icon with a blank message, and the status reads only \"<label>:  \""
plane: experiential
question: What does VcStatWidget show when errorText is only whitespace?
questions:
  - text: Does a blank errorText put VcStatWidget into the error state?
  - text: What does VcStatWidget announce for a whitespace errorText?
status: active
appliesTo:
  - axis: component
    value: vcstatwidget
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /story/components-organisms-vcstatwidget--with-suffix
evidence:
  - method: observation
    deployment: vcst_qa
    build: 63a7907dd6e47437
    conditions: theme=2.61.0-pr-2568-3f8a-3f8af63a; storybook=2.61.0-alpha.2594-vcst-5903-vc-stat-widget-3f8af63a; browser=firefox
    defect: suspected
    at: 2026-10-19T11:02:02.913Z
    by: session:43a7a721
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 63a7907dd6e47437
    conditions: storybook=2.61.0-alpha.2594-vcst-5903-vc-stat-widget-3f8af63a; theme=2.61.0-pr-2568-3f8a-3f8af63a; commit=vc-frontend@3f8af63a; browser=chromium
    at: 2026-10-19T15:43:46.302Z
    by: session:43a7a721
    bodyHash: e03f1929c994
    defect: suspected
    who: Lenajava1
    note: "Re-observed on chromium, With Suffix story: errorText \" \" gives a 92px card with the head and the alert icon only (y74), and the status a11y name is \"Active carts:\". errorText \"\" leaves the ready card (\"34 items\"). Source vc-stat-widget.vue:17 v-else-if=\"errorText\" and :96 use plain truthiness."
  - method: observation
    deployment: vcst_qa
    build: 4661c4a61799a028
    conditions: storybook=2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676; theme=2.61.0-pr-2568-bfe0-bfe0b259; commit=vc-frontend@bfe0b259; browser=chromium
    at: 2026-10-20T12:34:44.317Z
    by: session:43a7a721
    bodyHash: e03f1929c994
    who: Lenajava1
    contradicts: true
    contested: storybook 2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676 vs 2.61.0-alpha.2594-vcst-5903-vc-stat-widget-3f8af63a; theme 2.61.0-pr-2568-bfe0-bfe0b259 vs 2.61.0-pr-2568-3f8a-3f8af63a; browser chromium vs firefox; theme 2.61.0-pr-2568-bfe0-bfe0b259 vs 2.61.0-pr-2568-3f8a-3f8af63a
    note: "Fixed on a later build (VCST-6427, vc-frontend@bfe0b259, not yet released): hasError = !!errorText?.trim(), so errorText \" \" leaves the ready card (figures, description, delta), no alert icon, status empty; \"Boom\" still shows the error row and \"<label>: Boom\". The entry holds for 3f8af63a only. Remaining gap: a zero-width-only errorText (U+200B) is still an error with a blank message — KB-3F700CCC, VCST-6449."
---
QA Storybook (storefront UI kit), With Suffix story in the manager, errorText control set via "Set string" and then one space typed (URL arg errorText:+). The card drops the figure, suffix, description and delta. It shows only the head and the red alert icon, with an error row whose text is " ", at 51px from the top in a 92px card. The status output reads "Active carts:  " (label, colon, nothing). An empty errorText ("") leaves the card ready, because "" is falsy. The PR's value rule treats undefined and "" as missing but does not mention whitespace, and the error branch has the same gap.
