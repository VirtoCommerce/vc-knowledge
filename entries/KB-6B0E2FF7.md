---
id: KB-6B0E2FF7
subject: Storefront interactive controls at 375px are mostly 24-44 CSS px (UI-kit button tiers)
plane: experiential
question: What size are interactive controls on the storefront catalog page at a 375px mobile viewport?
questions:
  - text: Are the buttons on the catalog page big enough to tap comfortably on a phone?
  - text: What touch target sizes do storefront catalog controls have at a narrow mobile viewport for a target-size audit?
  - text: Which interactive controls on the mobile catalog page fall below 24 CSS pixels?
  - text: Do storefront catalog control heights follow the UI kit button size tiers on small screens?
concepts:
  - id: mobile-layout
  - id: accessibility
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /catalog
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T08:39:57.756Z
    by: session:p33116
    who: Lenajava1
---
At a 375px viewport, as a guest on the catalog page, almost all interactive controls (grid/list view toggles, quantity-stepper buttons, add-to-compare, header cart/search/phone links) measure between 24 and 44 CSS px, matching the vc-frontend UI kit button size tiers xxs/xs/sm (26/32/38 px). Only the hamburger menu button reached 44px or more. No actionable control below 24px was found; the few under 24px were visually hidden skip links or disabled decorative rating stars.
