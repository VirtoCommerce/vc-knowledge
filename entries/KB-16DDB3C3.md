---
id: KB-16DDB3C3
subject: With SiteBuilder.General.StoreUrl set, every store's Site Builder designer previews that one storefront and uploads its schema catalog for the store being designed
plane: experiential
question: Which storefront does the Site Builder designer preview for a store whose own URL does not load?
questions:
  - text: What does GET /api/site-builder/designer/storefront return when the module StoreUrl setting is set?
  - text: Does opening the designer write a schema catalog for the store?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: admin-designer
anchors:
  - coordinate: GET /api/site-builder/designer/storefront
  - coordinate: PUT /api/site-builder/catalog
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=QA-STORE; setting:SiteBuilder.General.StoreUrl=set
    defect: suspected
    at: 2026-11-19T20:11:55.211Z
    by: session:07a3812a
    who: Lenajava1
    note: "distinct: KB-620E2625 was observed with SiteBuilder.General.StoreUrl unset (preview fails, saves refused); with the module-level StoreUrl set the same store previews another store's storefront and gets that storefront's catalog written for it."
---
Module setting SiteBuilder.General.StoreUrl = the B2B storefront URL. GET /api/site-builder/designer/storefront?storeId=QA-STORE (whose own Store URL is an unreachable example.com host) returned {url:<that storefront>, previewPath:'/designer-preview'}. Opening the QA-STORE designer from a page loaded the B2B-store storefront in the preview (B2B-store logo and menus) and sent GET /api/site-builder/catalog?storeId=QA-STORE&hash=96e0af86 -> 404 then PUT /api/site-builder/catalog?storeId=QA-STORE -> 200, so QA-STORE now holds the B2B storefront's catalog (GET catalog 404 before, 200 after). No route removes a store's catalog.
