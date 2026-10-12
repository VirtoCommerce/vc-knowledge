---
id: KB-3F1DBB6E
subject: Admin product search returns imgSrc only when responseGroup includes ItemAssets; ItemInfo alone omits imgSrc and images
plane: experiential
question: Does POST /api/catalog/search/products with responseGroup ItemInfo return the product's imgSrc?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/catalog/search/products
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-12T22:46:16.289Z
    by: session:e201271b
    who: Aleksandra-Mitricheva
---
2026-10-13, admin token, POST /api/catalog/search/products {take:5, sort:'code:asc'}. With responseGroup 'ItemInfo' every item had imgSrc and images absent (undefined). With responseGroup 'ItemInfo,ItemAssets' the same 5 items carried imgSrc (an absolute https URL to the product's primary image) and images[] (1-2 entries, images[0].url == imgSrc). A seeder that needs a product image URL (e.g. to set an order line item's imageUrl) must request ItemAssets.
