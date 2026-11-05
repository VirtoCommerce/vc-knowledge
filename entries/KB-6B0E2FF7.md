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
    deployment: vcst_qa
    at: 2026-09-28T08:39:57.756Z
    by: session:p33116
    who: Lenajava1
    build: 6b8b88f2e92e4dd8
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: theme=2.59.0; store=B2B-store; role=anonymous; viewport=375x812
    at: 2026-11-05T07:24:51.399Z
    by: session:35ec50d2
    bodyHash: c9a5e1f2845b
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.59.0 vs 2.59.0-pr-2501-7e0c-7e0cb485; virtocommerce.catalog 3.1048.2 vs 3.1046.0-pr-909-2839; virtocommerce.fileexperienceapi 3.1006.0 vs 3.1004.0; virtocommerce.pages 3.1010.0 vs 3.1008.0; virtocommerce.profileexperienceapimodule 3.1020.0 vs 3.1019.0; …
    note: "Guest, /catalog at 375x812, theme 2.59.0: actionable TEXT LINKS measure below 24px tall — breadcrumb Home 37.2x17, 'Show on a separate page' 277x16 (VcProductButton link, min-h-4 text-xs), one-line product title links 277x16.4 (VcProductTitle text-sm, leading 1.17em). So 'no actionable control below 24px' does not hold for links. All of them keep >=8px from every other target, so they satisfy the WCAG 2.2 SC 2.5.8 spacing exception — not a 2.5.8 violation. The 18x18 rating star is disabled (read-only VcRating), as the entry says. The 24-44px button tiers part of the entry holds."
---
At a 375px viewport, as a guest on the catalog page, almost all interactive controls (grid/list view toggles, quantity-stepper buttons, add-to-compare, header cart/search/phone links) measure between 24 and 44 CSS px, matching the vc-frontend UI kit button size tiers xxs/xs/sm (26/32/38 px). Only the hamburger menu button reached 44px or more. No actionable control below 24px was found; the few under 24px were visually hidden skip links or disabled decorative rating stars.
