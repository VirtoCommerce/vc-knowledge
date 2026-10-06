---
id: KB-DCE536F0
subject: Admin loyalty mission blade hides server validation messages and blanks the mission tree after a failed save
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
  - method: observation
    deployment: vcst
    at: 2026-09-29T13:52:11.707Z
    by: session:vcst6086
    who: Lenajava1
    contradicts: true
    note: "On platform 3.1074.0-pr-3125 (vc-platform PR 3125) the setError 'reading join' TypeError is gone: 7 rejected POST/PUT /api/loyalty-missions saves logged no TypeError, and both the blade header and View details show the server errorMessage ('Mission reward amount cannot be negative'). The API still returns 400 with the bare FluentValidation array. The console TypeError in this entry holds only on platform builds without that PR."
    splitFrom: KB-E84AAF1D
  - method: observation
    deployment: vcst
    at: 2026-09-29T13:43:32.254Z
    by: session:p64772
    who: Lenajava1
    contradicts: true
    note: "On platform 3.1074.0-pr-3125 with VirtoCommerce.Loyalty 3.1009.0-pr-18: 4 POST and 3 PUT saves with a negative FixedAmountReward still return 400 with a bare FluentValidation array and persist nothing. The mission blade header and View details both show 'Mission reward amount cannot be negative'. The platform setError 'reading join' TypeError is no longer logged; the only console line per save is the browser's own 'Failed to load resource: 400'. The TypeError note in the earlier dispute holds only on platform builds without vc-platform PR 3125."
    splitFrom: KB-E84AAF1D
---
The Admin SPA mission details blade does not surface errorMessage: on create (POST, no error callback) the blade header shows only '400: Bad request', View details opens an empty Error details dialog, and the platform setError throws 'Cannot read properties of undefined (reading join)' in the console. On update (PUT) the header shows only 'Error 400'. In both cases the mission expression tree (conditions, goal, reward) renders blank after the failed save because the UI information is stripped from the entity before the request and not restored. On create, confirming the Error details dialog re-runs GET /api/loyalty-missions/new and discards everything the admin entered.
