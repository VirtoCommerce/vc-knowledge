---
id: KB-DCE536F0
subject: "Admin loyalty mission blade: up to Loyalty 3.1008 it hid server validation messages and blanked the mission tree after a failed save; from Loyalty 3.1009 with platform 3.1074 it shows the message and keeps the form"
plane: experiential
question: What does the Admin loyalty mission blade show when a save is rejected with validation errors?
questions:
  - text: Why does the loyalty mission screen just say Bad request without telling me what is wrong?
  - text: After a rejected mission save in the back office, are the conditions, goal and reward I entered still there?
  - text: Does dismissing the error dialog on a new mission reload a blank mission and lose my input?
  - text: Is the 'reading join' TypeError in the console on a failed mission save a platform setError bug?
  - text: Does the mission blade header or View details show the server's validation errorMessage?
concepts:
  - id: loyalty-mission
  - id: admin-validation
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/loyalty-missions
  - coordinate: PUT /api/loyalty-missions
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-25T15:17:15.429Z
    by: session:p36116
    who: Lenajava1
    splitFrom: KB-E84AAF1D
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T16:11:11.361Z
    by: session:p84900
    who: Lenajava1
    contradicts: true
    note: Fixed on VirtoCommerce.Loyalty 3.1009.0-pr-18 (PR #18). POST and PUT /api/loyalty-missions still return 400 with the FluentValidation array. The Admin mission details blade now shows the server errorMessage in the blade header, for example 'Mission reward amount cannot be negative' and 'Mission goal value cannot be negative', and View details shows the same text. The expression tree (condition, goal, reward, Add links, entered values) stays rendered after the failed save. Clicking OK, Cancel or Dismiss on the error does not re-request /new and does not clear the form. The platform setError 'reading join' TypeError is still logged in the console on both POST and PUT 400s. The old behaviour holds only on builds without that PR.
    splitFrom: KB-E84AAF1D
    resolved: version-scoped
    resolvedAt: 2026-10-08T10:30:13.913Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/loyalty-missions-20261008
    resolution: Loyalty 3.1009.0 (PR #18) shows the server message and keeps the tree; re-observed live on 3.1009.0 2026-10-08. The body now names the builds on each side.
  - method: observation
    deployment: vcst
    at: 2026-09-29T13:52:11.707Z
    by: session:vcst6086
    who: Lenajava1
    contradicts: true
    note: "On platform 3.1074.0-pr-3125 (vc-platform PR 3125) the setError 'reading join' TypeError is gone: 7 rejected POST/PUT /api/loyalty-missions saves logged no TypeError, and both the blade header and View details show the server errorMessage ('Mission reward amount cannot be negative'). The API still returns 400 with the bare FluentValidation array. The console TypeError in this entry holds only on platform builds without that PR."
    splitFrom: KB-E84AAF1D
    resolved: version-scoped
    resolvedAt: 2026-10-08T10:30:14.755Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/loyalty-missions-20261008
    resolution: The setError TypeError is gone from platform 3.1074.0 (PR #3125); re-observed live on 3.1076.0 2026-10-08. The body now names the builds on each side.
  - method: observation
    deployment: vcst
    at: 2026-09-29T13:43:32.254Z
    by: session:p64772
    who: Lenajava1
    contradicts: true
    note: "On platform 3.1074.0-pr-3125 with VirtoCommerce.Loyalty 3.1009.0-pr-18: 4 POST and 3 PUT saves with a negative FixedAmountReward still return 400 with a bare FluentValidation array and persist nothing. The mission blade header and View details both show 'Mission reward amount cannot be negative'. The platform setError 'reading join' TypeError is no longer logged; the only console line per save is the browser's own 'Failed to load resource: 400'. The TypeError note in the earlier dispute holds only on platform builds without vc-platform PR 3125."
    splitFrom: KB-E84AAF1D
    resolved: version-scoped
    resolvedAt: 2026-10-08T10:30:15.627Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/loyalty-missions-20261008
    resolution: On Loyalty 3.1009.0 with platform >= 3.1074.0 the blade shows the message and logs no TypeError; re-observed live 2026-10-08. The body now names the builds.
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.Loyalty=3.1009.0; module:VirtoCommerce.Marketing=3.1009.0; store=B2B-store; role=Administrator
    at: 2026-10-08T10:30:10.470Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Admin blade 2026-10-08, create with a negative reward, create with a negative order-count goal, edit with a negative reward, edit with a negative order-count goal: the blade header shows the server errorMessage with View details / Dismiss, View details shows the same text, the expression tree and entered values stay rendered, OK and Dismiss neither re-request /api/loyalty-missions/new nor clear the form, and the console logs only the 400 resource line (no setError \"reading join\" TypeError). A negative OrderValue goal cannot be sent at all: the input has a min validator and Save disables."
---
What the Admin SPA loyalty mission details blade shows after POST or PUT /api/loyalty-missions is rejected with validation errors (the API's answer itself is KB-D64B1613) depends on two builds: the Loyalty module and the platform.

Up to Loyalty 3.1008 (observed 2026-09-25): the blade did not surface errorMessage. On create (POST, no error callback) the header showed only '400: Bad request', View details opened an empty Error details dialog, and the platform setError threw 'Cannot read properties of undefined (reading join)' in the console. On update (PUT) the header showed only 'Error 400'. In both cases the mission expression tree (conditions, goal, reward) rendered blank after the failed save, because the UI information was stripped from the entity before the request and not restored; on create, confirming the Error details dialog re-ran GET /api/loyalty-missions/new and discarded everything entered. No stand runs that build any more.

From Loyalty 3.1009.0 (vc-module-loyalty PR #18) the blade header shows the server's errorMessage ('Mission reward amount cannot be negative', 'Mission goal value cannot be negative') with View details / Dismiss, View details shows the same text, the expression tree and entered values stay rendered, and OK / Dismiss neither re-request /new nor clear the form. The console TypeError disappears only from platform 3.1074.0 (vc-platform PR #3125); on Loyalty 3.1009.0 with an older platform it was still logged. Observed live 2026-10-08 on Loyalty 3.1009.0 with platform 3.1076.0, on create and on edit. A negative OrderValue goal never reaches the server there: its input has a min validator and Save disables.
