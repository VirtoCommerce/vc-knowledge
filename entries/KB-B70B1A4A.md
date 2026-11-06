---
id: KB-B70B1A4A
subject: A collapsible VcWidget header button exposes no aria-expanded and no aria-controls in either state, in Storybook and on storefront facets
plane: experiential
question: Does the collapsible VcWidget header button expose its expanded/collapsed state (aria-expanded, aria-controls)?
questions:
  - text: Is aria-expanded set on the catalog facet filter header on /catalog?
  - text: What does a screen reader get from the VcWidget Collapsible story header?
  - text: Does button.vc-widget__header-container toggle aria-expanded when clicked?
status: active
appliesTo:
  - axis: component
    value: vcwidget
  - axis: surface
    value: storefront-ui
  - axis: tool
    value: storybook
anchors:
  - coordinate: /catalog
evidence:
  - method: observation
    deployment: vcst_qa
    build: 80aa52bbcc451dd4
    conditions: theme=2.62.0-pr-2620-b47e; platform=3.1094.0; setting:design=classic; browser=chromium
    defect: suspected
    at: 2026-11-06T11:50:11.533Z
    by: session:213be081
    who: Lenajava1
---
Chromium 1280, no sign-in. Storybook story components-organisms-vcwidget--collapsible on the PR build (vc-frontend PR 2620 @ b47e1167) and on the dev build: the header renders as button.vc-widget__header-container; before and after clicking it, aria-expanded and aria-controls are both absent (getAttribute null), while the body .vc-widget__slot-container switches display block -> none and has no id. The accessibility tree shows button "Widget title" with no expanded state in both states. On the storefront /catalog the facet headers (VcWidget size xs collapsible collapsed, e.g. Brand) behave the same: opening Brand shows its list but the tree shows no expanded state, whereas the header Language button on the same page does show [expanded] when open, so the snapshot does report the state when a control sets it. The VcWidget template binds no aria state on the header at base dev 37cdaf67 or at the PR head, so this predates the PR. Checked against BL-A11Y-004 / WCAG 2.2 SC 4.1.2 (state must be programmatically determinable).
