---
id: KB-DD59FBA4
subject: PUT /api/site-builder/catalog stores the storefront section catalog under the SHA-256 of its text and answers 400 with a text for an unmarked custom- key, a malformed kit or a body over 8 MB
plane: experiential
question: How is the storefront section catalog stored by PUT /api/site-builder/catalog, and what does it refuse?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/site-builder/catalog
  - coordinate: GET /api/site-builder/catalog
evidence:
  - method: observation
    deployment: vcst_dev
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-1129
    at: 2026-11-26T00:12:04.548Z
    by: session:c8a6829d
---
A PUT of the storefront's own catalog body answered 200 {hash: <64-hex SHA-256>}; the same bytes plus a trailing newline produced a different key. GET /api/site-builder/catalog returns a short 'hash' field from the storefront's body (e.g. be63a01e) that is NOT the storage key, and a theme save that echoes it answers 400 'there is no schema catalog'; publish_theme's dry run reports the real catalog_hash. Refused with 400 and a text (no 413/500): a section keyed custom-… not marked custom, kit not an object, kit tags not an object, broken JSON, bodies of 8.59 and 9.10 MB ('The catalog is larger than 8 MB').
