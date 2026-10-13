---
id: KB-AFF6282A
subject: Admin Return list shows Create date / Modify date as relative time for recent returns and as 'Mon d, yyyy h:mm:ss am/pm' for older ones
plane: experiential
question: What format do the Create date and Modify date cells of the Admin SPA Return list use?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /workspace/Return
  - coordinate: POST /api/return/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T17:02:56.269Z
    by: session:62b7421e
    who: Lenajava1
    build: 2f9a5dce907b5872
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-12T09:46:10.590Z
    by: session:573e5582
    who: Lenajava1
    note: "Same on platform 3.1078.0 + Return 3.1005.0-pr-28-fb4f (Edge, 2026-10-12): a just-created return reads 'a few seconds ago' in both cells; returns ~20 minutes old read 'Oct 12, 2026 11:21:38 am'."
    build: 0bb1aea0269f4c1b
    buildFrom: deploy-history
---
On Admin SPA 3.1076.0 the Return list grid renders Create date and Modify date as relative text for recent returns ('a few seconds ago', '3 minutes ago', '9 minutes ago') and as an absolute 'Oct 8, 2026 6:47:56 pm' for returns roughly 10+ minutes old. The return details blade shows the absolute 'Oct 8, 2026 7:00:41 PM' for the same return. A test asserting the absolute format on a just-created return reads the relative form.
