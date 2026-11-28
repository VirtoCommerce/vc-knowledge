---
id: KB-E77AF0C3
subject: After a cross-catalog category move followed by an intra-catalog move, a product linked into a DESCENDANT of the moved category keeps a stale outline (missing the new parent) on both Catalog 3.1048.4 and 3.1055.0-alpha.2598
plane: experiential
question: Is the outline of a product linked into a child category refreshed when the parent category is moved twice (cross-catalog, then within the catalog)?
questions:
  - text: Why does GET /api/catalog/products/{id} show an outline without the new parent category after a category move?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: operation
    value: category-move-outline
  - axis: surface
    value: admin-api
anchors:
  - coordinate: POST /api/catalog/listentries/move
  - coordinate: GET /api/catalog/products/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 72d127ef4cdd9465
    conditions: platform=3.1106.0; module:VirtoCommerce.Catalog=3.1055.0-alpha.2598
    defect: suspected
    at: 2026-11-28T11:37:22.587Z
    by: session:25a33e28
    who: kutasinaelena
---
2026-11-28. Setup: catalogs Owner/Src/Dst, category C in Src with child C2, product P2 owned by Owner linked into C2, category D2 in Dst. POST /api/catalog/listentries/move C into Dst root, then move C under D2 (same catalog). GET /api/catalog/categories/{C2} outline = Dst>D2>C>C2 (correct), but GET /api/catalog/products/{P2}?respGroup=Full linked outline = Dst>C>C2>P2 (D2 missing), still stale 20 s later. A product linked directly into C does get Dst>D2>C>P. Seen on vcptcore (Catalog 3.1055.0-alpha.2598) and vcptcore_regression (Catalog 3.1048.4), so not introduced by VCST-6376. A single intra-catalog move without the preceding cross-catalog move refreshed the same outline correctly on vcptcore. Cause not established (cache or outline build); not checked after a cache reset or reindex.
