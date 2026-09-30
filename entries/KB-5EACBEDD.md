---
id: KB-5EACBEDD
subject: "Sales-rep list: Stop sharing keeps the link key; re-share does not restore recipients"
plane: experiential
question: After a sales rep stops sharing a list (scope Private) and shares it again, is the link the same and are old recipients restored?
questions:
  - text: If I stop sharing a list and share it again, do my customers get a new link?
  - text: After a sales rep stops sharing a list and re-shares it, are the old recipients and message restored?
  - text: What does the shared wishlist query return to a reader after sharing is stopped versus after their organization is removed?
  - text: Which share scopes show a toast on save and which save silently?
  - text: Does the list Rename dialog show any sharing details?
concepts:
  - id: list-sharing
  - id: wishlist
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeWishlist
  - coordinate: Query.sharedWishlist
  - coordinate: /account/lists
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T11:44:44.139Z
    by: session:p2012
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T11:44:42.634Z
    by: session:p36640
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T12:26:36.575Z
    by: session:p40192
    who: Aleksandra-Mitricheva
    note: "Stop sharing then re-share to TechFlow: same key, recipients+message empty on reopen, former recipient /403"
---
Stop sharing (Share dialog Private + 'Stop sharing this list?' confirm) sets scope Private, clears targets and message; readers get Query.sharedWishlist = null with no error (an org removed from a still-shared list instead gets errors[] 'Access denied.' Forbidden). Re-sharing reuses the same sharing key/URL; previous recipients and message are not restored. The Rename dialog holds only List name + Description (no sharing details). No toast after Stop sharing or after saving Anyone-with-link / My organization, while Specific-customers saves toast 'List shared with N customers.' / 'List saved. Now shared with N customers.'
