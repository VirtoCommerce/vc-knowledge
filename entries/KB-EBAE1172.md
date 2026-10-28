---
id: KB-EBAE1172
subject: The account dashboard 'Monthly spend report' widget shows fixed Budget/Totally spent figures with no period label that do not follow the buyer's orders
plane: experiential
question: What do Budget and Totally spent in the /account/dashboard Monthly spend report widget reflect?
questions:
  - text: Is the dashboard spend widget computed from orders?
  - text: Why does Monthly spend report not match order history?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605
    at: 2026-10-28T20:07:33.885Z
    by: session:bb892225
    who: Lenajava1
---
On /account/dashboard the 'Monthly spend report' widget showed Budget $58,152 and Totally spent $530,152 with no period label (no 'This Month'); the amounts did not correspond to the buyer's orders in /account/orders for the current month. The triage classifier, reading vc-frontend client-app/pages/account/dashboard.vue, reported these values as literals in the template with a static spend-chart.svg and no data binding, i.e. a placeholder widget. Observed by regression run REG-2026-10-28-1013 (suite 010, B2C-DASH-003/007).
