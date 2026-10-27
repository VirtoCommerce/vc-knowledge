---
id: KB-AC763A7D
subject: On /company/sales-reps the search applies only on Enter or the search button, only the Name column sorts (desc first, then asc), and the search input is capped at 64 characters
plane: experiential
question: How do search and sorting behave on the buyer-facing /company/sales-reps table?
questions:
  - text: Does the sales reps search filter while typing?
  - text: Which columns of the Sales reps table are sortable?
  - text: What is the max length of the sales reps search box?
status: active
appliesTo:
  - axis: actor
    value: buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/sales-reps
  - coordinate: Query.customerSalesReps
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: theme=2.61.0-pr-2576-be2c; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; store=B2B-store
    at: 2026-10-27T19:26:53.553Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
---
Signed in as a Purchasing agent: typing a keyword alone does not refetch; Enter or the "Search sales reps" button sends customerSalesReps with the keyword (case-insensitive substring match on name). A no-match keyword shows "No sales reps match your search" with a "Reset search" button, which clears the input and restores the full list. The Name header is a button: first click sends sort name:desc, second name:asc (default load is name:asc). Email and Phone headers are plain, clicking them sends nothing and leaves the order unchanged. Typing key-by-key accepts 64 characters and drops the 65th. Revisiting the page by SPA navigation refires customerSalesReps with byte-identical variables (first:10, after:"0", keyword:"", sort:"name:asc").
