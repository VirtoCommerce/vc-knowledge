---
id: KB-592EFFDF
subject: The Page Builder shell Assets Library "N assets" counter counts files only; subfolders are excluded
plane: experiential
question: Does the "N assets" counter in the Page Builder shell Assets Library include folders?
questions:
  - text: Why does a folder with subfolders show 0 assets in the Page Builder Assets Library?
  - text: What does fileCount vs totalCount mean in POST /api/page-builder-assets/search?
status: active
appliesTo:
  - axis: module
    value: page-builder
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: /apps/page-builder-shell#/page-builder-assets
  - coordinate: POST /api/page-builder-assets/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: d1ddf4bc127053db
    conditions: platform=3.1090.0-pr-3133; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-177-7157
    at: 2026-10-26T12:12:43.141Z
    by: session:92d8d5cf
    who: Lenajava1
    note: "distinct: KB-509C4B80 is about keyword retention on folder navigation and zero-hit copy; this is about what the toolbar asset counter counts (files vs folders)."
---
In the shell Assets Library (#/page-builder-assets), a folder holding two subfolders and no files shows "0 assets" while listing both folders; a folder with 22 files shows "22 assets" over two pages. POST /api/page-builder-assets/search returns both fileCount and totalCount (e.g. a keyword search matching one folder returned fileCount:0, totalCount:1); the toolbar counter follows fileCount, the paginator follows totalCount. The Designer picker counter behaves the same ("0 assets", status "1 – 2 of 2").
