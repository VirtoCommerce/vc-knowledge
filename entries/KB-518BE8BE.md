---
id: KB-518BE8BE
subject: Admin SPA ui-grid Grid Menu button floats over the header row and covers any header label that scrolls beneath it
plane: experiential
question: In an Admin SPA list grid that scrolls horizontally, does the floating grid-options (Grid Menu) button cover the header label under it, and is that specific to one module's grid?
status: active
appliesTo:
  - axis: component
    value: ui-grid-header
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /workspace/Return
  - coordinate: /workspace/security
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T21:51:33.955Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
Platform 3.1079.0-alpha.13411, Edge 154, fresh isolated browser context (no saved gridState), 1920x1080 and 1366x768. Every Admin SPA ui-grid gets the Grid Menu button (the platform enables it for all grids); it is a 26x26 px box, position absolute, right 15px, top 4px plus a 3px top margin, z-index 2, inside the grid, and the header row reserves no space for it. Return list (Return module 3.1005.0-pr-28-fb4f) with the two hidden columns Modify date and Created by switched on in the Grid Menu: nine columns at their minWidths make 1097 px in an 898 px body viewport, so the grid scrolls horizontally; at scrollLeft 0 the button covers 26x17 px of the 'Created by' header label (only 'C' and 'ed' stay readable, ' by' is past the viewport edge); scrolled to the end, every label is clear and 'Item count' ends 21 px before the button. The geometry is the same at 1366x768 because the blade has a fixed width. The platform's own Sign-in log list (Security, Sign-in log, View all records; fixed column widths of 1000 px in an 808 px viewport) behaves the same way: at scrollLeft 0 the button sits on the 'Type' header cell (its short label stays clear), and after a 70 px horizontal scroll it covers 13x17 px of the 'IP address' label. So which label is hidden depends only on which column is under the button's fixed x-range at the current scroll position. Every header keeps a title tooltip with the full label. Toggling columns sends no request (the grid state is kept in browser local storage).
