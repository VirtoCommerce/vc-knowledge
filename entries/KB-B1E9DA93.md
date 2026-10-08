---
id: KB-B1E9DA93
subject: The sales-rep hub dashboard "My activity" widget stays on skeleton placeholders for over 20 s after /company/dashboard opens, while /company/activities shows its badge counts within about 3 s
plane: experiential
question: How long does the My activity widget on the sales-rep dashboard take to load compared with the My activity page?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: user
    value: sales-rep-advanced
  - axis: viewport
    value: desktop-1920
anchors:
  - coordinate: /company/dashboard
  - coordinate: /company/activities
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T12:21:41.494Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
---
An Advanced sales rep who serves 15 orgs (theme 2.59.0-pr-2458, 1920px) opened Sales Rep hub > My activity (/company/activities). All six badge counts (All 156 = Orders 35 + Customers 15 + Searches 26 + Product views 55 + Logins 25, This year) were present in the first snapshot about 3 s after the click. She then opened Sales Rep hub > Dashboard (/company/dashboard). The "My activity" widget still showed 5 grey skeleton rows about 22 s after the page opened, and was populated (orders placed, sign-ins, per org) by about 45 s. The document title also stayed "Virto Commerce" on /company/dashboard instead of the "QA-audit-test | B2B-store | ..." pattern used on the other pages. This is a single observation and the cause was not isolated.
