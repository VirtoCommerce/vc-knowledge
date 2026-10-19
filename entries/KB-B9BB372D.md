---
id: KB-B9BB372D
subject: VcStatWidget shows the dash only for value undefined or empty string; a whitespace-only value renders an empty, zero-height figure row
plane: experiential
question: What does the VcStatWidget UI-kit card show when its value prop is missing, empty, whitespace-only or "0"?
questions:
  - text: Does VcStatWidget show a dash for a blank value?
  - text: Why does a stat card lose its figure line when value is a space?
  - text: VcStatWidget hasValue check for whitespace
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
evidence:
  - method: observation
    deployment: vcst_qa
    build: 63a7907dd6e47437
    conditions: theme=2.61.0-pr-2568; commit=vc-frontend@3f8af63a
    defect: suspected
    at: 2026-10-19T11:03:07.468Z
    by: session:43a7a721
    who: Lenajava1
---
Storybook (UI-kit) Components/Organisms/VcStatWidget Basic story, Controls: value undefined (args value:!undefined) and value "" both render div.vc-stat-widget__value--pending with the dash in neutral-300, 38px tall. value "0" renders 0 normally. value " " (one space) renders div.vc-stat-widget__value with only whitespace: no pending class, no dash, height 0, so the description moves up into the figure line (card height drops). Source at the PR head: hasValue = value !== undefined && value !== "" (no trim). Contradicts the PR's stated intent that no value shows the same dash.
