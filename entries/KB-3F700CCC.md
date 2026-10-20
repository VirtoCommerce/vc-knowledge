---
id: KB-3F700CCC
subject: "VcStatWidget's blank guard (trim) does not catch zero-width characters: a U+200B value renders a blank figure row instead of the dash, and a U+200B errorText shows the error state with an empty message"
plane: experiential
question: Does the VcStatWidget card show the dash when value or errorText contains only a zero-width space (U+200B)?
questions:
  - text: What does VcStatWidget render for a value that is only invisible characters?
  - text: Is a zero-width space treated as blank by VcStatWidget hasValue / hasError?
  - text: Why does VcStatWidget show an error icon with no message?
  - text: Which characters does the VcStatWidget empty-value check ignore?
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
    conditions: storybook=2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676; commit=vc-frontend@bfe0b259; browser=chromium
    defect: suspected
    at: 2026-10-20T12:18:50.333Z
    by: session:43a7a721
    who: Lenajava1
    note: "distinct: Existing entries cover ASCII whitespace on the pre-fix build; this is zero-width/format characters on the post-VCST-6427 build where whitespace is already handled."
  - method: observation
    deployment: vcst_qa
    build: 4661c4a61799a028
    conditions: storybook=2.61.0-alpha.2597-vcst-5903-vc-stat-widget-a6255676; commit=vc-frontend@bfe0b259; browser=firefox
    at: 2026-10-20T12:34:57.793Z
    by: session:43a7a721
    bodyHash: fe90624a64f2
    defect: VCST-6449
    who: Lenajava1
    note: First seen on firefox in the exploratory session (value U+200B and " \t U+200B " blank row; errorText U+200B blank error); filed as VCST-6449.
---
QA Storybook (UI kit), Components/Organisms/VcStatWidget Basic and With Suffix stories, build of vc-frontend PR #2568 after the VCST-6427 fix (hasValue = !!value?.trim(), hasError = !!errorText?.trim()). Control: value of three spaces now renders the pending dash with an empty status, so the trim guard is live. Entered through the Controls textarea by keyboard and by clipboard paste: value U+200B renders the ready-state div.vc-stat-widget__value (not --pending), 37.5px tall and visually blank, no dash, status output empty; value " U+200B " in With Suffix shows only the unit "items". errorText U+200B takes the error branch: figures hidden, alert icon with a blank message, and output.vc-stat-widget__status reads "<label>: U+200B". Cause: String.prototype.trim removes only White_Space, and U+200B (also U+200C-200F, U+061C, U+2060) is a format character it keeps; U+00A0, U+202F, U+3000 and U+FEFF are removed. Reproduced 2/2 on chromium.
