---
id: KB-A6DBACE4
subject: "Calendar export of a new call: Google gets text \"Call — <customer>\", UTC start and +30 min end, notes as details; Apple gets call.ics with UID = task id and RFC 5545 escaping; a refused save closes the pre-opened tab"
plane: experiential
question: What does Add to calendar send to Google and Apple Calendar when a sales rep saves a new call, and what happens when the save fails?
questions:
  - text: What is inside the call.ics a sales rep downloads?
  - text: Does the Google Calendar tab open when saving a call in Firefox?
  - text: What does a rep see when a call cannot be saved because the customer was unassigned meanwhile?
status: active
appliesTo:
  - axis: module
    value: salesrep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: Mutation.createSalesRepTask
evidence:
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; theme=2.60.0; browser=firefox; timezone=UTC-6 (no DST)
    at: 2026-10-20T14:29:17.331Z
    by: session:70c763de
    who: yuskithedeveloper
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; module:VirtoCommerce.TaskManagement=3.1005.0; theme=2.60.0; browser=firefox; store=B2B-store; role=Sales Representative; state:org-unassigned-while-dialog-open=true
    at: 2026-10-20T18:06:53.975Z
    by: session:70c763de
    bodyHash: 35025d5fc6a1
    who: yuskithedeveloper
    note: "Scope of this confirmation: the refused-save half only (calendar export was not re-observed). Firefox, 1920 px, a freshly created Sales Representative serving two sample orgs; Schedule call opened on the second org's /company/my-customers/{id}, that org unassigned through PUT /api/sales-rep while the dialog was open. Save x3 (typed date; same dialog saved again; date picked with Google Calendar ticked): every createSalesRepTask returned HTTP 200 errors[0].message \"Customer not found.\" with no extensions code, the only feedback was the toast \"Something went wrong. Please try again later.\" with Report a problem, the dialog stayed open and filled with Save enabled and no inline message, only one browser tab remained after the Google-ticked attempt, and no task row was written. Re-assigning the org with the dialog still open and saving the same draft succeeded (\"Task saved\", dialog closed)."
---
In Firefox (default pop-up settings) Save with Google Calendar ticked opens a tab whose calendar render URL carries action=TEMPLATE, text "Call — <customer name>", dates <dueDate UTC>/<dueDate +30 min UTC> and details = Notes; an 11:45 PM call picked at UTC-6 gave 05:45Z of the next day, and the list showed it on the picked day. Apple Calendar downloads call.ics: CRLF lines, UID = the saved task id, DTSTART/DTEND in UTC (+30 min), SUMMARY "Call — <customer>" with the comma escaped, DESCRIPTION with ; , \ and newline escaped; long lines are not folded. When the org was unassigned while the dialog was open, Save (createSalesRepTask answers "Customer not found.") shows only the toast "Something went wrong. Please try again later.", keeps the dialog filled, closes the pre-opened tab and writes nothing; Save again after re-assignment succeeds and opens a fresh tab. A double-click on Save creates one call.
