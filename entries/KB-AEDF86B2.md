---
id: KB-AEDF86B2
subject: Admin store SEO list accepts and persists a second record with an identical slug for the same store and language, with no warning; /api/seoinfos/duplicates returns [] for that store
plane: experiential
question: Does the Admin SPA validate a duplicate slug for the same store and language in the store SEO list?
status: active
appliesTo:
  - axis: owner
    value: store
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: PUT /api/stores
  - coordinate: GET /api/seoinfos/duplicates
  - coordinate: /workspace/store
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-13T16:20:03.564Z
    by: session:f4689b85
    who: kutasinaelena
---
Seo 3.1007.0-pr-24-9025. Store with saved en-US record slug X; Add > en-US, slug X > OK (no inline warning, OK enabled) > store Save: PUT /api/stores 204, GET /api/stores/{id} returns two identical seoInfos. GET /api/seoinfos/duplicates?objectId={storeId}&objectType=Store returns []. Debug SEO links shows 2 candidates through Stage 5 and picks 1 at Stage 6.
