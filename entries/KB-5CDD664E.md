---
id: KB-5CDD664E
subject: A plain-HTML product-list whose cards carry only data-product-name/price (no id/sku) re-pushes view_item_list on every unrelated DOM change
plane: experiential
question: Does a markup product list built from data-product-* attributes without an id or sku send view_item_list once, or again on later DOM mutations?
status: active
appliesTo:
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /agent-test-ga4-html-product-lists
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486
    at: 2026-10-13T13:53:26.501Z
    by: session:aa76d86f
    who: Lenajava1
    build: 942f354b4df8bb56
    buildFrom: deploy-history
---
Guest, Chrome 1920, a Page Builder HTML page with two data-vc-track product-list containers: one whose cards carry data-product-id/sku/name/price and one whose cards carry only name and price. On load each pushed one view_item_list. Opening and closing the header All products menu and typing a phrase into the header search, then Escape, pushed 12 more view_item_list for the name+price list (13 total, each delivered as its own /g/collect en=view_item_list hit) while the id-carrying list stayed at 1. With no interaction for 10 s the count stayed put, so each DOM mutation triggers one re-send. Partial items carry no item_id key (item_name, affiliation, currency, price only). The PR's engine dedups by entity id (id ?? code); id-less items are fresh objects on each scan so the shallow comparison never matches.
