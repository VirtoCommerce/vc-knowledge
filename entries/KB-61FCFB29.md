---
id: KB-61FCFB29
subject: "Admin Shell Ctrl+K search: the empty-query listbox holds scope-chip buttons and no options, and a second Escape leaves focus on the document body"
plane: experiential
question: What roles does the Admin Shell Ctrl+K search popover expose, and where does focus go on Escape?
questions:
  - text: Is the Admin Shell global search accessible to screen readers?
  - text: Where does focus go after closing the Admin Shell Ctrl+K search?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: viewport
    value: desktop
anchors:
  - coordinate: /apps/admin-shell#/
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; browser=edge
    defect: suspected
    at: 2026-10-23T13:17:03.226Z
    by: session:5de4e186
    who: kutasinaelena
---
At 1920 px, as an administrator, I pressed Ctrl+K with no query. The accessibility tree shows listbox "Search results" containing 8 scope buttons (Menu, Settings, Users, Roles, Orders, Customers, Products, Stores) and a help paragraph, with no option elements. After typing a query the results are options inside named groups, but user and role results are options without an accessible name. The first Escape clears the query and keeps focus in the combobox. The second Escape closes the popover and no element is focused (body), even when a dashboard element had focus before Ctrl+K.
