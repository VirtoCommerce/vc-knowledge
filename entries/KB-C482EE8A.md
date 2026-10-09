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
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T21:32:16.479Z
    by: session:61a7b05a
    who: yuskithedeveloper
    note: Re-observed independently on the same build (theme 2.60.0-pr-2523-466f, Return 3.1005.0-pr-28-fb4f) on Edge. A Requested return stored with the owner's user id in UPPERCASE (admin PUT /api/return) shows "Requested by <owner's own name>" and no Cancel return, both through SPA navigation from My returns and through a direct load of /account/returns/{id}. The page's own GetReturn response carries cancel isAvailable=true, and the client's user id from GetPageContext is lower case. A New return stored the same way shows the same Requested-by row and no Cancel footer, where a lower-case own New return shows a disabled Cancel return.
---
Return 3.1005.0-pr-28-fb4f + theme 2.60.0-pr-2523-466f, 2026-10-09. An admin PUT /api/return keeps a customerId sent in the body as is (snapshots fill only empty fields), so a return can be stored with the buyer's user id in UPPERCASE. For such a Requested return the server treats the buyer as the owner (case-insensitive): return(id) answers with availableActions cancel isAvailable=true, and the buyer's returns list includes it. The storefront page /account/returns/{id}, signed in as that same buyer, renders the colleague variant: a "Requested by <buyer's own name>" row and NO "Cancel return" button (a normal-case own Requested return shows Cancel return and no Requested by). The client compares customerId with the user id case-sensitively.
