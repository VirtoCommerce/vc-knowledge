---
id: KB-1A7FBDA2
subject: "Sales rep customer profile Quick actions: 'Schedule call' tile is enabled and opens a 'Schedule call' task popup (VCST-6314); only 'Order on behalf' and 'Create quote' are disabled"
plane: experiential
question: Is the Schedule call tile on the sales rep customer profile Quick actions disabled or does it do something?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; browser=firefox
    at: 2026-10-28T09:46:33.211Z
    by: session:7e8ea7fb
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: theme=2.62.0-pr-2596-e749-e749b47f; module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; browser=edge; role=Sales Representative
    at: 2026-11-03T16:49:52.205Z
    by: session:c3b82fae
    bodyHash: a0f2eb0a282c
    who: Lenajava1
    note: "Quick actions on a served customer page: Order on behalf and Create quote disabled, Send email and Schedule call enabled. Schedule call dialog opened (not submitted): Title Call, Customer prefilled and clearable, Due date empty, Time 9:00 AM, Priority Normal, Notes 0/1000, Add to calendar Google Calendar + Apple Calendar, Cancel, Save disabled; no Type field."
---
On /company/my-customers/<guid> as a sales rep (theme 2.61.0-alpha.2605), the Quick actions widget shows four tiles; 'Order on behalf' and 'Create quote' render disabled, 'Send email' and 'Schedule call' are enabled. Clicking 'Schedule call' opens a modal titled 'Schedule call' with Title (default 'Call'), Customer (pre-filled with the profile org), Due date, Time, Priority, Notes, Add to calendar (Google/Apple) and Cancel/Save. A case that expects all three non-email tiles disabled no longer matches.
