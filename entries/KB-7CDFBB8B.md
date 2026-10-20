---
id: KB-7CDFBB8B
subject: "Desktop /account/orders Filters popover: the Created date preset list opens on keyboard focus and stays open after Tab, hiding the focused Start date field and its calendar button"
plane: experiential
question: On /account/orders at desktop width, does the Created date preset dropdown in the Filters popover close when keyboard focus moves on to the date fields?
questions:
  - text: Is the Start date field visible when tabbing through the desktop orders Filters popover?
  - text: Does the orders filter date preset select stay expanded after Tab on desktop?
  - text: Which orders filter control hides the Start date input for keyboard users?
status: active
appliesTo:
  - axis: feature
    value: orders-date-filter
  - axis: layout
    value: desktop-split
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: local
    conditions: browser=msedge-155; viewport=1440x900
    defect: suspected
    at: 2026-10-20T18:35:35.765Z
    by: session:70c763de
    who: yuskithedeveloper
    note: "distinct: Desktop Filters popover (split layout) at 1440 px; KB-6FC8637A is the mobile Filters drawer at 375 px on another stand."
---
Edge 155, 1440x900, signed-in buyer account, /account/orders with the Filters button clicked (the panel 'Orders filters' takes focus). Tab 1 reaches the panel's Close button; Tab 2 focuses the 'Created date' combobox (value 'Custom date') and its list opens on focus (aria-expanded=true, five options: Custom date, Last day, Last week, Last month, Last year). Tab 3 moves focus to the 'Start date' input and Tab 4 to 'Open calendar: Start date'; at both stops the list is still open and document.elementFromPoint at the centre and four corners of the focused control returns the 'Last day' option (5/5), so the focused control and the whole date row are hidden. The list is not part of the panel's tab order, so Tab walks past it. This is the desktop split-layout popover; KB-6FC8637A describes the same behaviour in the mobile Filters drawer. Escape on the date row closes the panel and returns focus to Filters; nothing is applied.
