---
id: KB-B38AEE58
subject: The lists page badge collapses organization-scoped and world-readable lists into one Shared state
plane: experiential
question: Can a list owner tell an organization-shared list from a public one on the lists page?
questions:
  - text: On my lists page, how do I know whether a shared list is visible to my company or to everyone?
  - text: Which badge labels can the account lists page and add-to-list dialog show for a list's sharing state?
  - text: Does the lists roster badge distinguish organization, public and customer-shared scopes?
  - text: Where must a tester read a list's real scope when the roster badge only says Shared?
concepts:
  - id: wishlist
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /account/lists
  - coordinate: WishlistType.scope
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T07:46:06.085Z
    by: session:6341e00a
    splitFrom: KB-E367DA11
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:39:04.652Z
    by: session:p26336
    who: Lenajava1
    contradicts: true
    note: "Theme 2.59 on the rep's /account/lists: a Customer-scope list shows the badge 'Shared with customer', a third label besides Private; the badge is not limited to Private/Shared."
    splitFrom: KB-E367DA11
    resolved: claim-amended
    resolvedAt: 2026-10-12T17:25:49.091Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "The badge has three texts, not two: a Customer-scope list reads 'Shared with N customer(s)'; organization and Anyone-with-link lists still share 'Shared' and one icon, on vcst and vcptcore."
    amendedFrom: 1683d32e07ee
  - method: observation
    deployment: vcst
    conditions: theme=2.60.0-pr-2511-81f1; platform=3.1078.0; module:VirtoCommerce.XCart=3.1041.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.SalesRep=3.1013.0-pr-18-d367; store=B2B-store; role=sales-rep
    at: 2026-10-12T17:25:00.270Z
    by: session:34990ccd
    who: Dan-BV
    note: "/account/lists and the Add-to-list dialog, 2026-10-12: 'Private' with a lock icon; 'Shared with 1 customer' / 'Shared with 2 customers' for Specific customers; the same people icon and the same text 'Shared' for My organization and for Anyone with link."
    investigation: true
    bodyHash: 566851c10154
  - method: observation
    deployment: vcptcore
    conditions: theme=2.60.0-pr-2522-6f90; store=B2B-store; role=sales-rep
    at: 2026-10-12T17:25:01.928Z
    by: session:34990ccd
    who: Dan-BV
    note: "/account/lists and the Add-to-list dialog, 2026-10-12: 'Private' with a lock icon; 'Shared with 1 customer' / 'Shared with 2 customers' for Specific customers; the same people icon and the same text 'Shared' for My organization and for Anyone with link."
    investigation: true
    bodyHash: 566851c10154
---
On theme 2.60.0-pr-2511-81f1 (vcst) and 2.60.0-pr-2522-6f90 (vcptcore), 2026-10-12, /account/lists and the Add-to-list dialog show 'Private' (lock icon) for a private list and 'Shared with N customer(s)' for a Specific-customers (Customer-scope) list, but the same people icon and the same 'Shared' text for an organization-scoped list and for an Anyone-with-link list. So an owner cannot tell an organization-shared list from a world-readable one on the page where they would look; the scope is only legible in the Share dialog, one click deeper, or in WishlistType.scope.
