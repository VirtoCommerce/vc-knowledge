---
id: KB-C2DE7D28
subject: Sales-rep dashboard 'My recent orders' widget has status filter buttons (All plus one per status present) that narrow the table to that status
plane: experiential
question: How do the status filters on the sales-rep dashboard recent-orders widget behave?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /company/my-customers
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:56:17.138Z
    by: session:df9d131f
    who: Lenajava1
---
On theme vc-frontend-next 3.0.0-alpha.2685 with SalesRep 3.1013.0-pr-18, /company/dashboard for a rep serving 4 orgs shows a 'My recent orders' widget with toggle buttons All (pressed by default), Cancelled, New, Payment required, Processing and a table Order # / Customer / Date / Status / Total. Each button (aria-pressed) narrowed the table to rows of only that status (Cancelled 1, New 5, Payment required 1, Processing 2); no GraphQL errors. On /company/my-customers a non-matching name search shows 'No customers match your search' with a Reset search button.
