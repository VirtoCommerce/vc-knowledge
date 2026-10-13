---
id: KB-2E88BF7C
subject: "Store blade: after Save with the SEO list blade still open, deleting an SEO row there does not mark the store dirty (Save stays disabled, deletion not persisted)"
plane: experiential
question: In Admin SPA store SEO list, can I delete a record and Save without closing the SEO blade after a previous store Save?
status: active
appliesTo:
  - axis: owner
    value: store
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: PUT /api/stores
  - coordinate: /workspace/store
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-13T16:20:03.398Z
    by: session:f4689b85
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T17:59:15.267Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Platform 3.1076.1 (Stable 16): reproduced; deleting in a freshly opened SEO list (no store Save while it was open) does mark the store dirty and persists. Ticking the row enabled toolbar Delete."
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T19:34:40.260Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Store 3.1009.0 / Seo 3.1005.2: also holds for Add - after a store Save with the SEO list still open, Add + OK puts the row in the grid but store Save stays disabled; reopening the list restores the persisted state and Add/Delete then mark the store dirty."
---
Seo 3.1007.0-pr-24-9025, Store 3.1009.0. Stores > store > SEO widget > Add record > OK > store Save (PUT /api/stores 204), keep seo-list blade open, select row > Delete > Yes: row vanishes from the grid but store Save/Reset stay disabled and GET /api/stores/{id} still returns the record. seo-list keeps a reference to the pre-save store entity (blade.seoContainerObject; refresh() only replaces it when called with a new entity). Closing and reopening the SEO blade after the save makes Delete mark the store dirty and Save persists. The product blade does not show this: after product Save the open SEO list re-binds and Delete + Save persists.
