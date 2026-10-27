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
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; theme=2.60.0; browser=chrome; state:browser-timezone=UTC-6
    at: 2026-10-20T15:12:39.341Z
    by: session:70c763de
    bodyHash: 19e47acec486
    who: yuskithedeveloper
    note: "Same line 'Due Oct 23, 2026, 3:45 PM · Contoso Ltd. · Call' on /company/tasks. Layout: at 1440 px the Task column wraps it onto two lines ('... 3:45 PM ·' / 'Contoso Ltd. · Call'); at 768 and 375 px the card layout shows it on one line. The dashboard widget (separate surface) truncates the same string with an ellipsis, see the dashboard entry."
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; theme=2.60.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; module:VirtoCommerce.TaskManagement=3.1005.0; role=Sales Representative; state:browser-timezone=UTC-6; browser=chrome
    at: 2026-10-20T18:00:03.091Z
    by: session:70c763de
    bodyHash: 19e47acec486
    who: yuskithedeveloper
    note: "2026-10-20: on /company/tasks with Oct 23 picked in the page calendar, the call row reads 'Due Oct 23, 2026, 3:45 PM · Contoso Ltd. · Call'. The sub-line is display block with white-space normal and no line clamp. At 1440 px it wraps onto 2 lines (176 px wide) and is fully visible; at 375 px it fits on 1 line (245 px). The dashboard widget cuts the same string at both widths."
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; theme=2.60.0; role=Sales Representative; state:browser-timezone=UTC-6
    at: 2026-10-20T18:04:32.725Z
    by: session:70c763de
    bodyHash: 19e47acec486
    who: yuskithedeveloper
    note: "Edge, All tab: a task whose type was stored as \"CALL\" (case variant written through the API) gets the call line too, with the type printed verbatim: 'Due Nov 2, 2026, 11:15 AM · Contoso Ltd. · CALL' (dueDate 17:15Z, browser UTC-6); a task typed \"AGENT-TEST unlisted\" reads 'Due Nov 2, 2026 · AGENT-TEST unlisted' (no time, no customer). salesRepTasks returned type \"CALL\" and customerName for it."
  - method: observation
    deployment: vcptcore_qa
    build: 956cfff4cb0a606f
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; module:VirtoCommerce.TaskManagement=3.1005.0; theme=2.61.0-pr-2597; browser=firefox; state:browser-timezone=UTC+3
    at: 2026-10-27T10:48:59.363Z
    by: session:e75fe8a4
    bodyHash: 19e47acec486
    who: Aleksandra-Mitricheva
    note: "UTC+3, en-US: 'Due Oct 28, 2026, 2:30 PM · AGENT-TEST-Org-AcmeCorp-20260310 · Call' for dueDate 11:30Z (identical in the dashboard widget DOM), and a call saved with the Customer cleared (no customerId sent) reads 'Due Oct 29, 2026, 10:00 AM · Customer not assigned · Call'."
---
Theme 2.60.0 with the SalesRep plugin built from vc-module-sales-rep PR #26 (unreleased), browser zone UTC-6, en-US, 1920 px. Under a call's title the Tasks page row shows 'Due Oct 23, 2026, 2:30 PM · Contoso Ltd. · Call': the time is the stored UTC dueDate in the browser's zone (h:mm AM/PM) and the customer is the API's customerName. A call saved without a customer shows 'Due Oct 26, 2026, 10:15 AM · Customer not assigned · Call'; a non-call task shows 'Due Oct 24, 2026 · Other' with no time and no customer. The line wraps inside the table cell, so it stays fully readable. Picking a calendar day lists the tasks whose dueDate falls between that day's local midnight and +24h-1ms (Query.salesRepTasks period), so a call at 11:45 PM local is listed on its own day and a call at 12:00 AM on the next day; the calendar cell reads e.g. '1 task. Marked: Upcoming'.
