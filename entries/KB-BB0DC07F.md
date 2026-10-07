---
id: KB-BB0DC07F
subject: On a Collapsed store sharing a virtual catalog, explain rejects a linked category's store-less record with NoSeoPath while the Long-links store resolves it
plane: experiential
question: Why does a linked category not resolve on a store with Collapsed links, and what does explain say?
status: active
appliesTo:
  - axis: build
    value: catalog-pr-910
  - axis: module
    value: catalog
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/seoinfos/explain
  - coordinate: Query.categories
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T21:52:16.835Z
    by: session:f9a8159e
---
A category from a physical catalog linked at the root of a virtual catalog, with one store-less en-US record, resolves on the store with Long links (7 stages, 1 item) but on a second store with default Collapsed links and the same virtual catalog the Candidates stage rejects it with NoSeoPath only, and xAPI categories returns slug null for it there. A level-2 linked category whose parent's record is assigned to the other store gets NoSeoPath on the Collapsed store and PermalinkMismatch (details = parent/child path) on the Long store. Products: outside the store catalog -> NotInStoreCatalog; bare slug of a nested product -> PermalinkMismatch with every full path, comma-separated; product whose category has no slug -> NoSeoPath. Stage 0 counts each record (4 categories sharing one slug -> 4).
