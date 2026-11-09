---
id: KB-2B78C4DB
subject: On /account/dashboard edit mode, the My recent orders 'Max rows' spinbutton clamps out-of-range input on blur (0 becomes 1, 21 becomes 20) with no invalid state and Save left enabled
plane: experiential
question: What happens when a buyer types 0 or 21 into 'Max rows' of My recent orders on the account dashboard?
questions:
  - text: Does the account dashboard Max rows field show a validation error for values outside 1..20?
  - text: Can an out-of-range recent-orders row count be saved on /account/dashboard?
status: active
appliesTo:
  - axis: role
    value: b2b-buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/dashboard
  - coordinate: Query.layout
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: c185459e7260c4a3
    conditions: theme=2.62.0-pr-2552-ec75
    at: 2026-11-09T10:35:12.059Z
    by: session:9fb93ecf
    who: kutasinaelena
    note: "distinct: KB-5D6EC340 is the Sales Rep dashboard /company/dashboard (salesRepLayout); this is the buyer account dashboard /account/dashboard (layout scope accountDashboard)."
---
Signed in as a B2B buyer, Edit layout on /account/dashboard, spinbutton 'Max rows for My recent orders, between 1 and 20': typed 0 then Tab -> value became 1; typed 21 then Tab -> value became 20. No invalid flag or message appeared and 'Save layout' stayed enabled. Saving 1 and 20 stored recent_orders maxRows 1 / 20 in layout(scope accountDashboard) and the list rendered min(setting, orders) rows after reload; the list did not refetch while typing. So an out-of-range value is never stored, but it is corrected silently rather than refused.
