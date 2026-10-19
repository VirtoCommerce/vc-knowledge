---
id: KB-D7AF72F3
subject: VcStatWidget isolates its loader overlay (isolation:isolate on a z-auto relative root), so the z-996 overlay cannot rise above the storefront's z-20 sticky header
plane: experiential
question: Can a loading VcStatWidget's spinner overlay paint over a sticky header?
questions:
  - text: What z-index does the VcStatWidget loader overlay have and is it contained?
  - text: Does the storefront sticky header stay above a loading stat card?
status: active
appliesTo:
  - axis: component
    value: vcstatwidget
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /story/components-organisms-vcstatwidget--loading
evidence:
  - method: observation
    deployment: vcst_qa
    build: 63a7907dd6e47437
    conditions: theme=2.61.0-pr-2568-3f8a-3f8af63a; storybook=2.61.0-alpha.2594-vcst-5903-vc-stat-widget-3f8af63a; browser=firefox
    at: 2026-10-19T11:01:45.128Z
    by: session:43a7a721
    who: Lenajava1
---
QA Storybook (storefront UI kit) story components-organisms-vcstatwidget--loading: the card root computes position relative, z-index auto, isolation isolate, overflow visible; the inner div.vc-loader-overlay is absolute with z-index 996. The isolate root creates a stacking context, so 996 is ranked only inside the card. Outside it, the card paints at level 0 among positioned siblings. On the vcst_qa storefront home page the sticky header wrapper (div.sticky top-0 z-20) computes z-index 20, so it stays above a loading card that scrolls under it. Without the isolate rule, 996 would beat 20. The scroll-under situation itself was not built live: no storefront page uses the card yet, and DOM injection is outside the real-user rule. A sticky element with z-index auto placed earlier in the DOM would still be painted under the card, as with any positioned card.
