---
id: KB-CF3D4DCD
subject: In the admin Search index blade, Swap indices is a context-menu action on the backup row, applied immediately without confirmation
plane: experiential
question: Where is the swap-indexes action in the admin Search index blade and does it ask for confirmation?
questions:
  - text: How do I swap active and backup search indexes in the Virto admin?
  - text: Does Swap indices show a confirmation dialog?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/search/indexes/swapIndex
  - coordinate: GET /api/search/indexes
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: module:VirtoCommerce.ElasticSearch8=3.1012.0-pr-47-d205; platform=3.1093.0-pr-3154
    at: 2026-11-03T20:20:10.196Z
    by: session:ef5e0d1c
    who: Lenajava1
---
The Search index blade toolbar has only Refresh, Build index (dialog: Build | Delete and build | Cancel) and a Show/Hide backup indices toggle. With backups shown, each backup row's context (three-dot) menu offers 'Swap indices' and 'Value mapping'. Clicking Swap indices swapped the active and backup Product indexes at once with no confirmation dialog: the grid relabelled the rows (counts unchanged) without a page reload, GET /api/search/indexes returned the other index as active, and the xAPI store total changed accordingly. Repeated swaps (all 7 document types) worked the same way.
