---
id: KB-9FDE89B8
subject: a personalization tag on a virtual category hides its whole subtree from customers without that user group
plane: experiential
question: why does a storefront category page show no products while the index holds them (user_groups, /api/personalization/taggeditem)
questions:
  - text: Why does a category page show subcategories but no products to me as a regular shopper?
  - text: How does a tag on a virtual category hide its products from customers outside that customer group?
  - text: Why are products excluded from search even by exact SKU when their index document has user_groups set instead of __any?
  - text: How do I diagnose a personalization tag inherited by linked subcategories and products from a virtual category?
  - text: Which index field should I compare between a visible and a hidden product to spot a user-group restriction?
concepts:
  - id: personalization
  - id: user-group
  - id: search-index
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/personalization/taggeditem/{id}
  - coordinate: GET /api/search/indexes/index/Product/{id}
evidence:
  - method: observation
    deployment: virtostart
    at: 2026-09-25T19:57:52.018Z
    by: session:p3836
    who: Lenajava1
---
With catalog personalization installed, a tag set on a virtual category (GET /api/personalization/taggeditem/{categoryId} shows tags:[...]) is inherited by every linked subcategory and product (inheritedTags), and is written into each product's index document as user_groups:[<tag>] instead of __any. xAPI then excludes those products for anonymous shoppers and for any contact whose groups lack the tag, even when searching by productIds or the exact SKU. The category tree still lists the subcategories, so the page renders with categories but zero products. Diagnose by comparing user_groups in GET /api/search/indexes/index/Product/{id} against a product that does show.
