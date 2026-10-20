---
id: KB-7C95C0AE
subject: "VcStatWidget does not blank-check delta, description, label or valueSuffix: a whitespace or zero-width delta renders the trend icon with no text"
plane: experiential
question: "/story/components-organisms-vcstatwidget--basic VcStatWidget: what renders when delta, description, label or valueSuffix is whitespace-only or a zero-width space?"
questions:
  - text: Is the VcStatWidget delta prop trimmed like value and errorText?
  - text: Why does a VcStatWidget card show a chevron with no delta text?
  - text: Which VcStatWidget string props are blank-safe?
status: active
appliesTo:
  - axis: component
    value: vcstatwidget
  - axis: host
    value: storybook
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /story/components-organisms-vcstatwidget--basic
  - coordinate: /story/components-organisms-vcstatwidget--with-suffix
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4661c4a61799a028
    conditions: storybook=2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676; commit=vc-frontend@bfe0b259; browser=firefox
    defect: suspected
    at: 2026-10-20T12:15:17.994Z
    by: session:43a7a721
    who: Lenajava1
---
QA Storybook (UI kit) VcStatWidget Basic / With Suffix / Error State stories, props set through Controls. Only value and errorText are blank-checked (trim(), since the VCST-6427 fix); delta, description, label and valueSuffix are gated by a plain truthy v-if. delta " " or U+200B (with deltaIcon set) renders the delta row with the chevron icon and no text; description " " renders a 0-height node and U+200B a blank 14px line; label U+200B leaves an empty 28px head row; valueSuffix " " or U+200B renders a 0-width span. The card keeps its ready height with empty lines. Separately, trim() keeps U+200B, so value/errorText of only zero-width characters are still treated as content.
