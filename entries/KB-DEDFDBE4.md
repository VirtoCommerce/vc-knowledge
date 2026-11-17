---
id: KB-DEDFDBE4
subject: The /cart VcLineItems footer shows its desktop button ("Remove selected") only when the .vc-line-items container is wider than 42rem (672px); at 672px or less the mobile button ("Remove all") shows instead
plane: experiential
question: Which VcLineItems footer button (mobile / desktop) shows at which container width on /cart?
questions:
  - text: Why does the cart footer say Remove all on one width and Remove selected on another?
  - text: What is the VcLineItems footer button breakpoint?
  - text: At which viewport does the cart switch to the desktop Remove selected button?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 34600862b1f93b74
    conditions: theme=2.62.0-pr-2639-cfc1-cfc1d874; role=organization member
    at: 2026-11-16T16:19:35.705Z
    by: session:19efaaaf
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa
    build: 34600862b1f93b74
    conditions: theme=2.62.0-pr-2639-cfc1-cfc1d874; platform=3.1099.0; role=personal customer
    at: 2026-11-17T11:16:56.313Z
    by: session:1abcd6fc
    bodyHash: a7c38ba0caf5
    who: kutasinaelena
    note: "Re-measured as a signed-in personal customer: 671.2px (viewport 732) mobile only, 672.8px (733) and 1038.4px (1920) desktop only, 339.2px (400) mobile only; print: 0 of 23-25 .vc-button displayed at 733/1920/400."
---
Served CSS (vc.kit): .vc-line-items is the container (container-type: inline-size); .vc-line-items__button--desktop is display:none by default and display:inline-block inside @container (width > 42rem); .vc-line-items__button--mobile is display:none inside that query. Measured on /cart (Chromium, signed-in B2B user, 4 lines): container 371px and 400.8px -> mobile 'Remove all' shown, desktop none; 671.2px (viewport 732) -> mobile shown; 672.8px (viewport 733-734) and 1038px (viewport 1920) -> desktop 'Remove selected' shown (computed display block as a flex item), mobile none. Exactly one at each width. Both carry margin-inline-end 4px. In print media both are display:none.
