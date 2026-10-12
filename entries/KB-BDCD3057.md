---
id: KB-BDCD3057
subject: Admin 'Add new return' creates a return in status New that the buyer sees on /account/returns
plane: experiential
question: what status does a return created from the Admin SPA Returns 'Add new return' blade get, and does the buyer see it
questions:
  - text: If an admin creates a product return for a customer in the back office, does the customer see it in their account?
  - text: /workspace/Return Add new return status New legacy
  - text: What statuses does the status dropdown offer on a return created through Admin 'Add new return'?
  - text: Does an admin-created return appear on /account/returns next to Requested and Partially approved returns?
concepts:
  - id: return
  - id: return-status
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /workspace/Return
  - coordinate: /account/returns
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T15:02:59.704Z
    by: session:0cfc9f97
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T18:57:06.992Z
    by: session:0cfc9f97
    who: kutasinaelena
    note: "2026-10-01 Admin SPA: Add new return > order > line > Make return created RET in status New immediately; dropdown New, Completed, Cancelled, Processing; order in status New accepted."
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-02T06:03:46.790Z
    by: session:0cfc9f97
    who: kutasinaelena
    note: Admin Add new return on AGENT-TEST-ORD-RET-ADM073 created RET261002-00018 at status New immediately
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T17:02:46.720Z
    by: session:62b7421e
    who: Lenajava1
    note: "Admin SPA 3.1076.0: Add new return on AGENT-TEST-ORD-RET-ADM073 created RET261008-00082 at status New immediately; dropdown offers Completed, New, Canceled, Processing (label and stored status spelled 'Canceled', REST status 'Canceled', not 'Cancelled')."
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:33:54.990Z
    by: session:61a7b05a
    who: yuskithedeveloper
    note: "Admin SPA platform 3.1079.0-alpha.13411, Return 3.1005.0-pr-28-fb4f: Add new return > order > tick the line > Make return (PUT /api/return 200) created the return in status New at once; the buyer's xAPI returns lists it as New (the storefront page itself was not opened by this lane); no email or push."
---
In VirtoCommerce.Return 3.1003.0-pr-27, Admin SPA Returns > Add new return > pick order > select line > Make return creates the return with status New (legacy status). Its status dropdown offers New, Completed, Cancelled, Processing. The return shows up for the order's buyer on storefront /account/returns with status 'New', next to new-flow returns (Requested/Partially approved).
