---
id: KB-C075BDF9
subject: "reworked Lists menu: Rename edits name and description only; Share is its own dialog"
plane: experiential
question: where is list sharing configured on /account/lists after the sharing rework
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:54:38.407Z
    by: session:p25420
    who: Lenajava1
---
On vc-frontend#2476 (theme 2.59.0-pr-2476-0604) the list card Actions menu is Rename / Share / Remove list and the list page offers Rename and Share. Rename edits only name and description; sharing (scope, customers, message) lives only in the Share dialog. The earlier view-only settings dialog is gone.
