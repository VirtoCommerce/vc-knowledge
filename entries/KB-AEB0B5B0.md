---
id: KB-AEB0B5B0
subject: The Sales Statistics period and Last year toggles persist in browser localStorage under keys not scoped to the user, so the next user on the same browser inherits them; an invalid stored period falls back to Current year
plane: experiential
question: Where does the /account/dashboard Sales Statistics block remember the selected period and Last year toggles, and is that per user?
questions:
  - text: Does the Sales Statistics period survive a reload?
  - text: Does a second user on the same browser see the previous user's Sales Statistics period?
  - text: What happens when the stored Sales Statistics period is invalid?
status: active
appliesTo:
  - axis: role
    value: b2b-buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 775727ee41d78e49
    conditions: theme=2.62.0-pr-2625-ac8f; store=B2B-store
    at: 2026-11-11T06:52:20.925Z
    by: session:86a76b47
    who: Aleksandra-Mitricheva
---
2026-11-11, Chrome. Keys: vc-account-sales-report-period (e.g. 'q4'), vc-reporting-sales-last-year and vc-reporting-aov-last-year ('true'/'false'). Choosing Q4 and the Monthly sales Last year switch, then reloading, keeps both. Setting the period key to an unknown value and reloading shows Current year (the bad value stays stored). A no-org buyer chose Q2, signed out; an org buyer signing in on the same browser opened /account/dashboard on Q2. The layout itself (Query.layout) is per user server-side; only these view choices are per browser.
