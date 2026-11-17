---
id: KB-9BCF68BE
subject: Lowering ProductComparison.MaxItems below a category group's size trims what compareList and /compare show to the oldest N items without deleting the rest; raising it brings them back
plane: experiential
question: What does /compare show when ProductComparison.MaxItems is lowered below the number of products already in a category group?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
  - coordinate: Query.compareList
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: c2cbb357a76b771a
    conditions: theme=2.63.0-pr-2635-63a1; module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-98d6; store=B2B-store; setting:ProductComparison.MaxItems=3
    at: 2026-11-17T14:57:54.231Z
    by: session:fcdb91ae
    who: kutasinaelena
---
A buyer's group held 5 products; store setting ProductComparison.MaxItems set to 3: compareList and /compare return only the 3 oldest items of the group ("3 of 3 added", header badge 3); adding another product to that group shows the toast "Only 3 products from the same category can be compared" (the UI sends no request; the API refuses with COMPARE_LIST_LIMIT_REACHED). Setting MaxItems back to 5 shows all 5 again — the hidden items were not deleted, the cap is applied when the list is read.
