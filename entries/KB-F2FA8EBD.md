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
    deployment: vcptcore
    at: 2026-09-29T12:26:20.989Z
    by: session:p28404
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore
    at: 2026-10-02T08:45:27.428Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "Build 2.59.0-pr-2476-31b5 (2026-10-02): the 'Change who can access?' body now reads 'The link stays the same. Some users may lose access.' (the 'Everyone the list is shared with now will lose access.' copy is gone); the dialog still opens on Specific customers -> Anyone with link although nobody loses access. Transition matrix otherwise unchanged; see KB-91DF8883."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T15:23:00.097Z
    by: session:70a35285
    who: Lenajava1
    contradicts: true
    note: "Local theme 2.59.0-pr-2476-0abb over vcst_qa backend, 2026-10-05: Specific customers -> Anyone with link opens NO confirmation (3/3) and the 'now will lose access' copy is gone; 'Change who can access?' now reads 'The link stays the same. Some users may lose access.' and appears for narrowing and for removing a recipient inside Specific customers (no longer silent). Stop sharing copy and the guest 404 after Stop sharing unchanged. See KB-E6436922."
---
Theme 2.59.0-pr-2476: Specific customers -> Private shows 'Stop sharing this list?' (Cancel focused); Specific customers -> My organization or -> Anyone with link shows 'Change who can access?' with 'Everyone the list is shared with now will lose access.' even when the move widens access (former recipients and anonymous users still read the list); My organization -> Private also shows 'Stop sharing this list?'. No confirmation on first share or a recipient swap inside Specific customers. After Stop sharing, a former recipient opening the link gets the 404 page; after re-sharing to a different customer the same key is reused and the former recipient gets /403 Access denied. The recipient's own /account/lists never lists lists shared with them. Save is disabled when nothing changed.
