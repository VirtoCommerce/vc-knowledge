---
id: KB-A5C0B71C
subject: On the Storybook built from theme 2.62.0-pr-2639, VcButton stories render as plain text (no background, border, padding or kit font) while VcBadge in the same Storybook and VcButton on the same theme's storefront render styled
plane: experiential
question: Do VcButton stories render styled in the Storybook of the vc-frontend PR 2639 build (:where(.vc-button) base rules)?
questions:
  - text: Why does VcButton look unstyled in Storybook after the :where(.vc-button) change?
  - text: Is VcButton's colour and padding applied in the PR 2639 Storybook?
status: active
appliesTo:
  - axis: app
    value: storybook
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /story/components-molecules-vcbutton--basic
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 34600862b1f93b74
    conditions: theme=2.62.0-pr-2639-cfc1-cfc1d874; browser=firefox; storybook-preset=default; design=classic
    defect: suspected
    at: 2026-11-16T16:26:33.379Z
    by: session:19efaaaf
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa
    build: b1f4805fadeecd22
    conditions: theme=2.62.0-pr-2639-cfc1-cfc1d874; browser=chromium+firefox; storybook-preset=default; design=classic
    at: 2026-11-16T19:19:54.161Z
    by: session:19efaaaf
    bodyHash: 18e145e5c7b7
    who: kutasinaelena
    contradicts: true
    contested: browser chromium+firefox vs firefox
    note: "Re-checked the same story (components-molecules-vcbutton--basic) on the same build: in Chromium the PR Storybook and the base Storybook render VcButton Basic identically (orange #f99e24 fill, 117.5x43.2, padding 0 16px, Lato). In Firefox 6 of 6 fresh loads also render it orange at 117.5 px. The only unstyled render (transparent background, black text, while padding and font from :where(.vc-button) did apply) came on the first load right after the stand had answered HTTP 503 for several minutes, i.e. a cold-start partial load, not a property of the :where() build. Static CSS layer order and --bg-color definitions are the same in both builds."
---
Storybook (kit component catalogue), not the shop. Firefox, 1280 and 1920 viewports, guest, Storybook manager view of story components-molecules-vcbutton--basic and --outline, waited until the canvas loading indicator was gone plus 10-25 s. VcButton Basic and Outline, and the VcPopupSidebar With Footer story (its Open sidebar / Apply / Reset VcButtons), show only black system-font text with no fill, border or padding; the Basic button box measured 96x40. In the same Storybook the VcBadge Basic story renders an orange filled badge, so the preset tokens are applied. The base dev Storybook (older theme) renders VcButton Basic as an orange filled button in the same browser. The storefront on the same theme renders VcButtons styled (search button orange, Change button filled). The served Storybook CSS contains :where(.vc-button){...background-color:var(--bg-color);padding...} and .vc-button--solid--primary{--bg-color:...} identical to the storefront's, and no static rule targeting button elements in vc.* or later layers was found, so the cause is not in the static CSS; not checked in Chromium.
