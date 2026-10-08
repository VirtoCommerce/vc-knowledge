---
id: KB-FE02F89F
subject: The All filter on /company/tasks paginates at 15 rows per page (22 tasks -> page 1 of 2) and the filter selection is not reflected in the URL
plane: experiential
question: How many rows does the All view on /company/tasks show per page, and does the selected filter tab survive navigation?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T09:36:11.185Z
    by: session:ae74f8af
    who: Lenajava1
---
Theme 2.60.0-pr-2536 on vcst_qa, sales rep with 22 tasks. Clicking the All filter tab on /company/tasks renders a heading "All / 22 tasks", a desktop table with 15 rows, and Previous / 2 / Next pagination under it. The URL path and query do not change when the filter tab is switched, so reloading or changing the locale prefix returns to the default Today tab.
