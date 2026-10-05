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
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:06.201Z
    by: session:p20808
    who: Aleksandra-Mitricheva
    note: Rename save sends ChangeWishlist with listId/listName/description only; sharing (scope, targets, message, key) survives the rename.
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T12:21:20.786Z
    by: session:p31812
    who: Aleksandra-Mitricheva
    note: "theme 2.59.0-pr-2476-0604: card Actions = Rename / Share / Remove list; Share dialog separate"
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T12:26:18.700Z
    by: session:p5868
    who: Aleksandra-Mitricheva
    note: "theme 2.59.0-pr-2476-0604: card menu Rename/Share/Remove list; list page Rename+Share; no List settings action"
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:20:35.492Z
    by: session:91e50fbd
    who: Aleksandra-Mitricheva
    note: "theme 2.59.0-pr-2476-0abb, list OWNER: card Actions = Rename / Share / Remove list; list page = Rename + Share (no Remove list on the page). For a NON-owner the set shrinks to Rename only (separate entry)."
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T13:02:00.125Z
    by: session:bbd60c47
    who: Aleksandra-Mitricheva
---
On vc-frontend#2476 (theme 2.59.0-pr-2476-0604) the list card Actions menu is Rename / Share / Remove list and the list page offers Rename and Share. Rename edits only name and description; sharing (scope, customers, message) lives only in the Share dialog. The earlier view-only settings dialog is gone.
