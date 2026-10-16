---
id: KB-D7B0A4FD
subject: VcTileButton in Storybook shows a visible keyboard focus ring (thin blue outline from the global focus style) on button and link tiles in light and dark mode; disabled tiles are skipped by Tab
plane: experiential
question: Does the ui-kit VcTileButton show a visible focus indicator when reached with Tab, and are disabled tiles skipped?
questions:
  - text: Is there a focus ring on VcTileButton?
  - text: Does Tab skip disabled VcTileButton tiles?
  - text: Does VcTileButton define its own focus style?
status: active
appliesTo:
  - axis: component
    value: vctilebutton
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /iframe.html?id=components-molecules-vctilebutton--grid
  - coordinate: /iframe.html?id=components-molecules-vctilebutton--links
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 55b5699ab0d1fe2f
    conditions: theme=2.61.0-alpha.2589-vcst-5906-vc-tile-button-2024d7ad; browser=chromium; host=storybook
    at: 2026-10-16T13:34:59.558Z
    by: session:e78e77b2
    who: Aleksandra-Mitricheva
---
On the PR build of VcTileButton (Storybook stories Grid and Links, Chrome 1920x1080), pressing Tab from the page start focused each enabled tile in DOM order: the solid and outline button tiles in Grid, then the router-link and external-link tiles in Links. Each focused tile showed a thin blue outline around its border. The component defines no focus style of its own, so the outline comes from the global focus rule. The outline was visible in light mode and in dark mode (darkMode:dark global). The two disabled tiles in Grid (rendered as disabled buttons) were skipped, and the next Tab left the story. A mouse click put no ring on the tile. Enter on the router-link tile changed the URL to / through SPA navigation, without a page reload. Enter on the external-link tile opened a new tab. Observed in the ui-kit Storybook, not on a storefront page, since no storefront page uses the component yet.
