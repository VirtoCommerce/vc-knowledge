---
id: KB-F9D75EA0
subject: VcDateRangePicker footer Clear scope differs between split and combined layouts
plane: experiential
question: In the storefront date range picker, does the calendar footer Clear empty both dates or only one?
questions:
  - text: How do I clear both dates in the order history date filter?
  - text: Does the calendar footer Clear reset the whole range or only one field in the date range picker?
  - text: Why does the date range filter behave differently when clearing on a narrow screen?
  - text: What accessible names do the calendar triggers have in the split and combined date range layouts?
concepts:
  - id: date-range-picker
  - id: mobile-layout
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T15:28:41.618Z
    by: session:79182e15
    who: Lenajava1
---
In the split layout (two labelled fields, used at sm/640px and wider) each field opens its own calendar, and that calendar's footer Clear empties only its own field; emptying the range needs Clear in both calendars. In the combined layout (one Date range field, below sm) the single calendar's footer Clear empties both dates at once. The split fields carry no in-field clear button; their calendar triggers are named 'Open calendar: Start date' / 'Open calendar: End date', while the combined trigger is named just 'Open calendar'.
