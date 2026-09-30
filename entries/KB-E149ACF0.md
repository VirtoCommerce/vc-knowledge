---
id: KB-E149ACF0
subject: vc-shell blade stack keeps the hash route in sync on every close path
plane: experiential
question: Does closing a vc-shell blade (Esc, X, breadcrumb, mobile back, unsaved-changes confirm) update the URL?
questions:
  - text: If I close a panel in the vendor portal and refresh the page, does the closed panel come back?
  - text: Does the URL update when a blade is closed with Esc, the X button, a breadcrumb or the mobile back arrow?
  - text: Does cancelling the unsaved-changes dialog keep the blade and its route?
  - text: Do non-routable child blades appear in the hash route, and what does Esc close when a child is open?
concepts:
  - id: blade-navigation
  - id: vendor-portal
status: active
appliesTo:
  - axis: surface
    value: vendor-ui
anchors:
  - coordinate: /apps/vendor-portal
evidence:
  - method: observation
    deployment: vcmp_dev
    at: 2026-09-25T09:03:58.486Z
    by: session:p75228
    who: Lenajava1
---
On vc-shell framework 2.6.0-rc.1 the hash route mirrors the visible routable blade stack after every close path: Esc on a focused details blade, the X button, a breadcrumb menu item, the mobile back arrow, and Confirm in the unsaved-changes dialog all drop the closed blade from the URL, so a reload restores only the remaining blades. Cancel in the dialog keeps the blade and its URL. Non-routable child blades (e.g. an order's Shipping widget blade) never appear in the URL; Esc closes the blade that holds focus together with its children.
