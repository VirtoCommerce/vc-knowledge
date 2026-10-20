---
id: KB-434F1EBE
subject: On /company/tasks a call's line reads 'Due <date>, <time> · <customer> · Call', or 'Customer not assigned' without a customer; other tasks show date and type only
plane: experiential
question: How does /company/tasks show a call's time and customer in the task list?
questions:
  - text: What does a call row on the sales rep Tasks page show under the title?
  - text: What does the Tasks page show for a call with no customer?
concepts:
  - id: sales-rep
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Query.salesRepTasks
evidence:
  - method: observation
    deployment: local
    conditions: role=Sales Representative; state:browser-timezone=UTC-6
    at: 2026-10-20T14:31:11.295Z
    by: session:70c763de
    who: yuskithedeveloper
---
Theme 2.60.0 with the SalesRep plugin built from vc-module-sales-rep PR #26 (unreleased), browser zone UTC-6, en-US, 1920 px. Under a call's title the Tasks page row shows 'Due Oct 23, 2026, 2:30 PM · Contoso Ltd. · Call': the time is the stored UTC dueDate in the browser's zone (h:mm AM/PM) and the customer is the API's customerName. A call saved without a customer shows 'Due Oct 26, 2026, 10:15 AM · Customer not assigned · Call'; a non-call task shows 'Due Oct 24, 2026 · Other' with no time and no customer. The line wraps inside the table cell, so it stays fully readable. Picking a calendar day lists the tasks whose dueDate falls between that day's local midnight and +24h-1ms (Query.salesRepTasks period), so a call at 11:45 PM local is listed on its own day and a call at 12:00 AM on the next day; the calendar cell reads e.g. '1 task. Marked: Upcoming'.
