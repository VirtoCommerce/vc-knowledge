---
id: KB-5EACBEDD
subject: "Sales-rep list: Stop sharing keeps the link key; re-share does not restore recipients"
plane: experiential
question: After a sales rep stops sharing a list (scope Private) and shares it again, is the link the same and are old recipients restored?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
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
  - method: observation
    deployment: vcptcore
    at: 2026-10-02T08:44:54.764Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
    note: "2026-10-02: Stop sharing from Customer and from AnyoneAnonymous -> scope Private, targets [], every reader gets sharedWishlist null with no error; the same key is reused on re-share. Toast 'List shared with 1 customer.' on Specific customers, 'List saved. Now shared with 1 customer.' after removing a recipient, none after Anyone-with-link or Stop sharing."
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T13:02:00.086Z
    by: session:bbd60c47
    who: Aleksandra-Mitricheva
    note: "Theme 2.59.0-pr-2476-0abb: Stop sharing -> former target reader gets /shared-list 404 page; re-share to another org reuses the same key, recipients and message empty on reopen (0/250), former recipient /403. Toasts 'List shared with 1 customer.' / 'List shared with 2 customers.'; none after Anyone-with-link save."
---
Stop sharing (Share dialog Private + 'Stop sharing this list?' confirm) sets scope Private, clears targets and message; readers get Query.sharedWishlist = null with no error (an org removed from a still-shared list instead gets errors[] 'Access denied.' Forbidden). Re-sharing reuses the same sharing key/URL; previous recipients and message are not restored. The Rename dialog holds only List name + Description (no sharing details). No toast after Stop sharing or after saving Anyone-with-link / My organization, while Specific-customers saves toast 'List shared with N customers.' / 'List saved. Now shared with N customers.'
