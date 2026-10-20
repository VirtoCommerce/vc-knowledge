---
id: KB-EF9531FB
subject: "Schedule call on a served customer's page opens a call draft: localized title, that customer, no day, 9:00 AM, priority Normal, Notes 0/1000, both calendars unticked"
plane: experiential
question: What title, customer, date, time and priority does the Schedule call quick action open with on /company/my-customers/{orgId}?
questions:
  - text: Is the Schedule call quick action enabled in the sales rep hub?
  - text: What priority does a new sales-rep call default to?
  - text: Is the call title translated when the storefront language changes?
  - text: Can the Time field of a sales-rep call be emptied?
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
  - coordinate: /company/tasks
evidence:
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; module:VirtoCommerce.TaskManagement=3.1005.0; theme=2.60.0; browser=firefox
    at: 2026-10-20T14:29:17.203Z
    by: session:70c763de
    who: yuskithedeveloper
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; theme=2.60.0; browser=chrome; state:theme-preset=default
    at: 2026-10-20T15:12:39.063Z
    by: session:70c763de
    bodyHash: 763d474e603d
    who: yuskithedeveloper
    note: "Re-observed on Chrome 155 at 1440, 768 and 375 px (signed in as a served Sales Representative): the 2x2 Quick actions grid (tiles 149x96 / 215.5x96 / 147x96, both rows equal height) and the Schedule call dialog open exactly as described (Title Call, Customer = Contoso Ltd. clearable, Due date empty MM/DD/YYYY, Time 9:00 AM with required marker and aria-required=true, Priority Normal, Notes 0 / 1000, Google and Apple Calendar unticked, Save disabled until a day is typed or picked). The Time list has 96 options (12:00 AM to 11:45 PM); the dialog is a 564 px panel from 769 px up and full-screen at 768 px and below."
  - method: observation
    deployment: local
    conditions: theme=2.60.0; store=B2B-store; role=sales-rep; browser=chrome
    at: 2026-10-20T15:40:29.512Z
    by: session:70c763de
    bodyHash: 763d474e603d
    who: yuskithedeveloper
    note: "Scope of this confirmation: the Quick actions tile set and grid only - the Schedule call dialog was not opened in this run. On /company/my-customers/{id} for a served org, signed in as a sales representative: Order on behalf disabled (disabled attribute, faded primary fill; a click is not delivered, no navigation and no dialog), Send email enabled, Create quote disabled (greyed), Schedule call enabled (no disabled attribute, pointer cursor, same styling as Send email); 2x2 grid at 1920 px (tiles 149x96), at 768 px (216x96) and at 375 px (147x96). The tile label reads 'Schedule call'."
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; theme=2.60.0; browser=firefox; store=B2B-store; role=Sales Representative
    at: 2026-10-20T18:06:54.503Z
    by: session:70c763de
    bodyHash: 763d474e603d
    who: yuskithedeveloper
    note: "Scope of this confirmation: the dialog defaults in Firefox at 1920 px (localization, option lists and the 375 px grid not re-checked). Schedule call on a served org's /company/my-customers/{id} opened with Title \"Call\", Customer = that org (clearable), Due date empty, Time 9:00 AM, Priority Normal, Notes counter, Google and Apple Calendar unticked, Save disabled until a day was typed (10/27/2026) or picked from Open calendar; Cancel and reopen gave a fresh draft."
---
On /company/my-customers/{orgId} for a served org, Quick actions shows Order on behalf (disabled), Send email, Create quote (disabled) and Schedule call (enabled), as a 2x2 grid at 1920 px and at 375 px. Schedule call opens a dialog "Schedule call" with Title "Call", Customer = that org (clearable; the picker lists only the rep's served orgs), Due date empty, Time 9:00 AM (quarter-hour select from 12:00 AM; Backspace/Delete do not empty it, no clear button), Priority Normal (options Lowest, Low, Normal, High, Highest), Notes counter 0 / 1000, "Add to calendar" with Google Calendar and Apple Calendar unticked; Save is disabled until a day is picked. Cancel and reopen gives a fresh draft. In German the dialog is "Anruf planen" with title "Anruf" and time 09:00, in French "Planifier un appel" with title "Appel"; the saved task's name is the localized title while its type stays "Call". New task (Tasks page) is titled "New task", prefills today, and shows Customer, Time and Add to calendar only after Type = Call; Edit call has no Type field and no calendar options. createSalesRepTask with priority "Medium" is refused: "Unknown task priority 'Medium'."
