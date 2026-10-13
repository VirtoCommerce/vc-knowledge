---
id: KB-F2FA8EBD
subject: share-list confirmation copy and revoked-link states
plane: experiential
question: Which confirmation does the storefront Share dialog show per scope transition, and what does a revoked recipient see on /shared-list/{key}?
questions:
  - text: What happens when I open a shared list link after the owner stopped sharing it with me?
  - text: Which confirmation dialog appears when a list's sharing moves from specific customers to my organization or private?
  - text: Does the Share dialog warn that access will be lost even when the change actually widens access?
  - text: Is the same sharing key reused after re-sharing, and what does a former recipient get then?
  - text: When is Save disabled in the list Share dialog, and is there a confirmation on first share?
concepts:
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists
  - coordinate: /shared-list/{key}
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:26:20.989Z
    by: session:p28404
    who: Aleksandra-Mitricheva
    build: 7f4081223d65d3fa
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-02T08:45:27.428Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "Build 2.59.0-pr-2476-31b5 (2026-10-02): the 'Change who can access?' body now reads 'The link stays the same. Some users may lose access.' (the 'Everyone the list is shared with now will lose access.' copy is gone); the dialog still opens on Specific customers -> Anyone with link although nobody loses access. Transition matrix otherwise unchanged; see KB-91DF8883."
    resolved: version-scoped
    resolvedAt: 2026-10-12T17:25:40.806Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: The entry's copy and widening confirmation held on the early 2.59.0-pr-2476 build; VCST-6104 changed both (e4a4cf58), as both disputes saw, re-observed on 2.60.0 on vcst and vcptcore; the revoked-link states still hold.
    build: 6ad05bb5fe261366
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T15:23:00.097Z
    by: session:70a35285
    who: Lenajava1
    contradicts: true
    note: "Local theme 2.59.0-pr-2476-0abb over vcst_qa backend, 2026-10-05: Specific customers -> Anyone with link opens NO confirmation (3/3) and the 'now will lose access' copy is gone; 'Change who can access?' now reads 'The link stays the same. Some users may lose access.' and appears for narrowing and for removing a recipient inside Specific customers (no longer silent). Stop sharing copy and the guest 404 after Stop sharing unchanged. See KB-E6436922."
    resolved: version-scoped
    resolvedAt: 2026-10-12T17:25:42.569Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: The entry's copy and widening confirmation held on the early 2.59.0-pr-2476 build; VCST-6104 changed both (e4a4cf58), as both disputes saw, re-observed on 2.60.0 on vcst and vcptcore; the revoked-link states still hold.
    build: 59983c56933fe571
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511-81f1; platform=3.1078.0; module:VirtoCommerce.XCart=3.1041.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.SalesRep=3.1013.0-pr-18-d367; store=B2B-store; role=sales-rep
    at: 2026-10-12T17:24:53.475Z
    by: session:34990ccd
    who: Dan-BV
    note: Same confirmation matrix as KB-91DF8883 today; the old copy 'Everyone the list is shared with now will lose access.' is gone. After Stop sharing a former recipient gets the 404 'Page not found' page at /shared-list/<key>; after re-sharing to another customer the key is unchanged and the removed recipient is redirected to /403 'Access denied'. Share disabled until a change.
    investigation: true
    bodyHash: 12de7e3105b4
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    conditions: theme=2.60.0-pr-2522-6f90; store=B2B-store; role=sales-rep
    at: 2026-10-12T17:24:55.136Z
    by: session:34990ccd
    who: Dan-BV
    note: Same confirmation matrix and copy as on vcst; re-share kept the sharing key. Former-recipient pages not observed here (no target-org member could sign in).
    investigation: true
    bodyHash: 12de7e3105b4
    build: c55c1a124b302d3b
    buildFrom: deploy-history
---
On theme 2.60.0-pr-2511-81f1 (vcst, 2026-10-12): Specific customers -> Private shows 'Stop sharing this list?' with Cancel / Stop sharing. A scope change that may narrow access, and removing a recipient inside Specific customers, show 'Change who can access?' with 'The link stays the same. Some users may lose access.'; the earlier copy 'Everyone the list is shared with now will lose access.' is gone, and Specific customers -> Anyone with link saves with no confirmation (full matrix: KB-91DF8883). No confirmation on a first share or on adding a recipient. After Stop sharing, a former recipient opening /shared-list/<key> stays on that URL and gets the 404 'Page not found' page. After re-sharing to a different customer the same sharing key is reused, and a former recipient opening it is redirected to /403 'Access denied'. The recipient's own /account/lists never lists lists shared with them. Share stays disabled until something changes. On the earlier PR build 2.59.0-pr-2476 (2026-09-29) the swap copy read 'Everyone the list is shared with now will lose access.' and the widening swap was confirmed.
