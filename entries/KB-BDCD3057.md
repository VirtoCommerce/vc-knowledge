---
id: KB-BDCD3057
subject: Admin 'Add new return' creates a return in status New that the buyer sees on /account/returns
plane: experiential
question: what status does a return created from the Admin SPA Returns 'Add new return' blade get, and does the buyer see it
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: /workspace/Return
  - coordinate: /account/returns
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T15:02:59.704Z
    by: session:0cfc9f97
    who: kutasinaelena
---
In VirtoCommerce.Return 3.1003.0-pr-27, Admin SPA Returns > Add new return > pick order > select line > Make return creates the return with status New (legacy status). Its status dropdown offers New, Completed, Cancelled, Processing. The return shows up for the order's buyer on storefront /account/returns with status 'New', next to new-flow returns (Requested/Partially approved).
