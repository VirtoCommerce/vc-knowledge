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
---
QA Storybook (storefront UI kit), With Suffix story in the manager, errorText control set via "Set string" and then one space typed (URL arg errorText:+). The card drops the figure, suffix, description and delta. It shows only the head and the red alert icon, with an error row whose text is " ", at 51px from the top in a 92px card. The status output reads "Active carts:  " (label, colon, nothing). An empty errorText ("") leaves the card ready, because "" is falsy. The PR's value rule treats undefined and "" as missing but does not mention whitespace, and the error branch has the same gap.
