---
id: KB-E149ACF0
subject: vc-shell blade stack keeps the hash route in sync on every close path
plane: experiential
question: Does closing a vc-shell blade (Esc, X, breadcrumb, mobile back, unsaved-changes confirm) update the URL?
status: active
appliesTo:
  - axis: surface
    value: vendor-portal-ui
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
