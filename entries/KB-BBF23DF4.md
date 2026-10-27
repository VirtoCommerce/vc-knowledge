---
id: KB-BBF23DF4
subject: Opening another user's /account/quotes/{id}/edit shows an endless spinner and a 'Save changes? Would you like to save changes?' prompt before landing on /403
plane: experiential
question: What does the storefront show when a signed-in buyer opens the edit URL of a quote request that belongs to someone else?
questions:
  - text: Why does a Save changes dialog appear on a quote I cannot access?
  - text: Does /account/quotes/{id}/edit redirect to 403 for a foreign quote?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.quote
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/quotes/{id}/edit
  - coordinate: Query.quote
evidence:
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    conditions: theme=2.61.0-pr-2577-4c17-4c171a96; store=B2B-store
    defect: suspected
    at: 2026-10-27T22:03:23.920Z
    by: session:3e14e295
    who: Lenajava1
---
Buyer A created a Draft quote; buyer A signed out on its edit page, so sign-in got returnUrl=/account/quotes/{id}/edit, and buyer B (another org) signed in. The quote query answered errors[0] 'Access denied.' (code Forbidden) with data.quote null inside a 200. The page showed title 'Quote request #' (no number), a spinner in the content area and a modal 'Save changes — Would you like to save changes?' with No/Yes, although nothing was edited. Clicking No navigated to /403 'Access denied'. The server side holds (no data leaked); the unsaved-changes guard fires on the redirect away from a quote that never loaded.
