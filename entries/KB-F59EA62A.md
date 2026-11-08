---
id: KB-F59EA62A
subject: Mission progress bars on /account/missions (cards and order-mission modal) expose no progressbar role or value to assistive tech on the released theme
plane: experiential
question: Do mission card and order-mission modal progress bars on /account/missions expose role progressbar and a value to screen readers?
questions:
  - text: Is the missions progress bar accessible
  - text: Does the mission card percentage have aria-valuenow
  - text: order-mission-modal progress bar accessibility
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/missions
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7477bc6a09fab490
    conditions: theme=2.62.0-pr-2620-b47e-b47e1167; store=B2B-store
    defect: VCST-6143
    at: 2026-11-08T23:42:46.513Z
    by: session:6de07ff2
    who: Aleksandra-Mitricheva
---
Signed in as a loyalty buyer on /account/missions (theme 2.62.0-pr-2620), all 23 mission cards render the bar as div.mission-card__track > div.mission-card__bar (style width N%) with no role, no aria-valuenow/min/max, no aria-hidden, and no <progress>; the visible 'N%' span (.mission-card__percent) is not tied to the bar. The order-mission modal (div.order-mission-modal__track) is the same. The accessibility tree shows only a generic 'N%' text. axe-core 4.10.2 reports no Critical/Serious rule for this (only heading-order, moderate), so the gap is not caught by axe. Fix in vc-frontend PR #2591 (adds an sr-only native <progress> named by the title, aria-hidden on track and percent) verified on a local build of that PR.
