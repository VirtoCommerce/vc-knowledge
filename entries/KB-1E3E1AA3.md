---
id: KB-1E3E1AA3
subject: At 390 px the floating Refresh button on Admin Shell Agent tools covers part of a tool switch in the list (the 5th tool at the initial scroll)
plane: experiential
question: At 390 px on Admin Shell Agent tools (#/mcp-tools), does the floating Refresh button overlap the tool switches?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: viewport
    value: 390
anchors:
  - coordinate: /apps/admin-shell#/mcp-tools
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    defect: suspected
    at: 2026-11-10T20:31:58.150Z
    by: session:40688c0a
    who: Lenajava1
---
2026-11-10 ~20:23Z, viewport 390x844, light theme. #/mcp-tools renders tools as cards with a 36x20 switch at x=317; a fixed Refresh button (102x44 at 268,724) floats over the list. At the initial scroll position it covers the lower half of the 'Check pending migrations' switch (switch box 317,713,36x20), so that undersized target has 0 px spacing to another target (WCAG 2.2 SC 2.5.8 spacing exception not met there); every other visible switch keeps >= 32 px. Which switch is covered depends on scroll position. Disconnect buttons on #/mcp-agents are 110x32 and the page has no horizontal overflow.
