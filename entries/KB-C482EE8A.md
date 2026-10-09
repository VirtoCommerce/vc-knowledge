---
id: KB-C482EE8A
subject: "Storefront return details treat the owner as a colleague when the stored customerId differs from the user id only in letter case: \"Requested by\" is shown and Cancel return is hidden, while the server still offers cancel"
plane: experiential
question: What does the owner see on /account/returns/{id} when the return's customerId differs from their user id only in letter case?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /account/returns/{id}
  - coordinate: Query.return
  - coordinate: PUT /api/return
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:24:19.702Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
Return 3.1005.0-pr-28-fb4f + theme 2.60.0-pr-2523-466f, 2026-10-09. An admin PUT /api/return keeps a customerId sent in the body as is (snapshots fill only empty fields), so a return can be stored with the buyer's user id in UPPERCASE. For such a Requested return the server treats the buyer as the owner (case-insensitive): return(id) answers with availableActions cancel isAvailable=true, and the buyer's returns list includes it. The storefront page /account/returns/{id}, signed in as that same buyer, renders the colleague variant: a "Requested by <buyer's own name>" row and NO "Cancel return" button (a normal-case own Requested return shows Cancel return and no Requested by). The client compares customerId with the user id case-sensitively.
