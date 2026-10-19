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
  - method: observation
    deployment: vcst_qa
    build: 63a7907dd6e47437
    conditions: storybook=2.61.0-alpha.2594-vcst-5903-vc-stat-widget-3f8af63a; theme=2.61.0-pr-2568-3f8a-3f8af63a; commit=vc-frontend@3f8af63a; browser=chromium
    at: 2026-10-19T15:43:46.031Z
    by: session:43a7a721
    bodyHash: 2ff0222a679c
    defect: suspected
    who: Lenajava1
    note: "Re-observed on chromium: Basic value \" \" and \"   \" give card 156->118px, description y111->73 (only 38px lost, so the 0-height figure div stays mounted); \"\" shows the dash, \"0\" shows 0. With Suffix value \" \" shows the unit alone (\"items\") in the 38px figure row, because the suffix span lives in the has-value branch. Source vc-stat-widget.vue:90 hasValue has no trim()."
---
Storybook (UI-kit) Components/Organisms/VcStatWidget Basic story, Controls: value undefined (args value:!undefined) and value "" both render div.vc-stat-widget__value--pending with the dash in neutral-300, 38px tall. value "0" renders 0 normally. value " " (one space) renders div.vc-stat-widget__value with only whitespace: no pending class, no dash, height 0, so the description moves up into the figure line (card height drops). Source at the PR head: hasValue = value !== undefined && value !== "" (no trim). Contradicts the PR's stated intent that no value shows the same dash.
