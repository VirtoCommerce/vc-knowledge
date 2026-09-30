---
id: KB-7CC25D4A
subject: A Draft row in the storefront returns list opens the /edit wizard step, which has no cancel/discard control; Cancel return exists only on /account/returns/:id
plane: experiential
question: how does a buyer discard a draft return on the storefront
questions:
  - text: How do I throw away a return I started but never submitted?
  - text: Why is there no cancel option when a buyer opens a draft return from the returns list?
  - text: Which storefront return page offers the Cancel return button for a draft?
  - text: Do draft return fields such as reason, comment and attachment persist via autosave?
concepts:
  - id: return
status: active
appliesTo:
  - axis: theme
    value: 2.59
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns
  - coordinate: /account/returns/{}/edit
  - coordinate: /account/returns/{}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-29T07:56:50.466Z
    by: session:p14200
    who: kutasinaelena
---
On theme 2.59 pr-2500, clicking a Draft row in /account/returns navigates to /account/returns/:id/edit ('Add details to return'), which shows details fields, Saved automatically and Submit return but no Cancel/Delete. The draft's /account/returns/:id page (reached by URL) shows an enabled Cancel return button with a confirm dialog. The header Back button on the /edit step is history-back. Draft fields (reason, comment, attachment) persist via autosave. Viewports 375 and 768.
