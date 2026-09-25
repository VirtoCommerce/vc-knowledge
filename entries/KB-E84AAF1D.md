---
id: KB-E84AAF1D
subject: Admin loyalty mission blade never shows server-side validation messages
plane: experiential
question: What does the Admin SPA loyalty mission blade show when POST or PUT /api/loyalty-missions returns 400 with validation errors?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/loyalty-missions
  - coordinate: PUT /api/loyalty-missions
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-25T15:17:15.429Z
    by: session:p36116
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T16:11:11.361Z
    by: session:p84900
    who: Lenajava1
    contradicts: true
    note: Fixed on VirtoCommerce.Loyalty 3.1009.0-pr-18 (PR #18). POST and PUT /api/loyalty-missions still return 400 with the FluentValidation array. The Admin mission details blade now shows the server errorMessage in the blade header, for example 'Mission reward amount cannot be negative' and 'Mission goal value cannot be negative', and View details shows the same text. The expression tree (condition, goal, reward, Add links, entered values) stays rendered after the failed save. Clicking OK, Cancel or Dismiss on the error does not re-request /new and does not clear the form. The platform setError 'reading join' TypeError is still logged in the console on both POST and PUT 400s. The old behaviour holds only on builds without that PR.
---
POST and PUT /api/loyalty-missions return 400 with a raw array of FluentValidation errors (propertyName, errorMessage). The Admin SPA mission details blade does not surface errorMessage: on create (POST, no error callback) the blade header shows only '400: Bad request', View details opens an empty Error details dialog, and the platform setError throws 'Cannot read properties of undefined (reading join)' in the console. On update (PUT) the header shows only 'Error 400'. In both cases the mission expression tree (conditions, goal, reward) renders blank after the failed save because the UI information is stripped from the entity before the request and not restored. On create, confirming the Error details dialog re-runs GET /api/loyalty-missions/new and discards everything the admin entered. Nothing is persisted on a 400.
