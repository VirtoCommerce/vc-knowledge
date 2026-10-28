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
---
On /company/my-customers/<guid> as a sales rep (theme 2.61.0-alpha.2605), the Quick actions widget shows four tiles; 'Order on behalf' and 'Create quote' render disabled, 'Send email' and 'Schedule call' are enabled. Clicking 'Schedule call' opens a modal titled 'Schedule call' with Title (default 'Call'), Customer (pre-filled with the profile org), Due date, Time, Priority, Notes, Add to calendar (Google/Apple) and Cancel/Save. A case that expects all three non-email tiles disabled no longer matches.
