---
id: KB-2FECA0AC
subject: /company/calendar renders the host 404 'Page not found' page for a signed-in sales rep with no redirect to /company/tasks; the hub rail Tasks item and the dashboard 'All tasks' link both point at /company/tasks
plane: experiential
question: What does a signed-in sales rep get at the old /company/calendar route after the Calendar-to-Tasks rename, and where do the rail Tasks item and the dashboard All tasks link point?
questions:
  - text: Does /company/calendar redirect to /company/tasks?
  - text: Is there any leftover Calendar link in the Sales Rep hub after the Tasks rename?
  - text: What href does the dashboard 'All tasks' link on /company/dashboard have?
  - text: Is /company/calendar an alias of the Tasks page?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/calendar
  - coordinate: /company/tasks
  - coordinate: /company/dashboard
evidence:
  - method: observation
    deployment: local
    conditions: role=Sales Representative; theme=2.60.0; state:served-organizations=4
    at: 2026-10-20T15:39:29.244Z
    by: session:70c763de
    who: yuskithedeveloper
    note: "distinct: KB-A6E1D965 is a transient 404 painted on live hub routes for a non-rep or anonymous visitor before the guard redirects; this is a signed-in sales rep on the removed /company/calendar route, where the 404 is final, nothing redirects, and the rail and dashboard links now target /company/tasks."
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: theme=2.61.0-pr-2576-be2c; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; browser=firefox
    at: 2026-10-27T20:20:21.301Z
    by: session:e75fe8a4
    bodyHash: c6464c687c0f
    who: Aleksandra-Mitricheva
---
Signed in as a seeded sales rep serving 4 organizations, Edge 1440x1080, theme 2.60.0 (footer label), Sales Rep hub from the SalesRep storefront plugin. On /company/dashboard the 'Tasks & due dates' widget's 'All tasks' link has href /company/tasks, and the 'Sales Rep hub' rail lists Dashboard, My customers, Tasks with the Tasks item at href /company/tasks. Clicking that rail item and loading /company/tasks directly both stay at /company/tasks with h1 'Tasks' and breadcrumb Home / Account / Sales Rep hub / Tasks. Loading /company/calendar directly keeps that URL and renders the host 404 page: h1 '404', h2 'Page not found', text 'An error occurred. Should we take you back to home page?', document.title 'B2B-store 404 Page not found'. There is no redirect and the Tasks page is not shown. None of the 40 links on /company/dashboard (header, rail, widgets, footer) has an href containing /company/calendar.
